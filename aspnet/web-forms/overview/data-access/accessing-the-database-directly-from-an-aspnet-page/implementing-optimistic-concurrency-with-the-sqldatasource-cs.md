---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
title: Implementowanie optymistycznej współbieżności przy użyciu kontrolki SqlDataSource (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziemy Przejrzyj podstawowych mechanizmu kontroli optymistycznej współbieżności, a następnie zobacz, jak wdrożyć je przy użyciu kontrolki SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: df999966-ac48-460e-b82b-4877a57d6ab9
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f2590e8e7712d719eb89403ef839f03066a93d2b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754050"
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-c"></a>Implementowanie optymistycznej współbieżności przy użyciu kontrolki SqlDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_CS.exe) lub [Pobierz plik PDF](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/datatutorial50cs1.pdf)

> W tym samouczku będziemy Przejrzyj podstawowych mechanizmu kontroli optymistycznej współbieżności, a następnie zobacz, jak wdrożyć je przy użyciu kontrolki SqlDataSource.


## <a name="introduction"></a>Wprowadzenie

Jak dodać Wstawianie, aktualizowanie i usuwanie możliwości kontrolki SqlDataSource zbadaliśmy w poprzednim samouczku. Krótko mówiąc, aby zapewnić te funkcje Musieliśmy określ odpowiedni `INSERT`, `UPDATE`, lub `DELETE` instrukcję SQL w formancie s `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` właściwości wraz z odpowiednim Parametry w `InsertParameters`, `UpdateParameters`, i `DeleteParameters` kolekcji. Podczas tych właściwości i kolekcje można określić ręcznie skonfigurować źródło danych Kreatora s Zaawansowana oferuje Generuj `INSERT`, `UPDATE`, i `DELETE` na podstawie wyboru instrukcji, która spowoduje automatyczne tworzenie tych instrukcji `SELECT` instrukcji.

Wraz z Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru, okno dialogowe Zaawansowane opcje generowania SQL zawiera opcję Użyj optymistycznej współbieżności (patrz rysunek 1). Po zaznaczeniu tej opcji, `WHERE` klauzul wygenerowany automatycznie `UPDATE` i `DELETE` instrukcje są modyfikowane w celu wykonania tylko aktualizacji lub usuwania Jeśli bazowego t nie zostały danych bazy danych został zmodyfikowany od czasu użytkownika ostatniego załadowania danych do siatki.


![Obsługa optymistycznej współbieżności można dodać z zaawansowanych generowanie kodu SQL — okno dialogowe Opcje](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.gif)

**Rysunek 1**: Obsługa optymistycznej współbieżności można dodać z zaawansowanych generowanie kodu SQL — okno dialogowe Opcje


Ponownie [Implementowanie optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) zbadaliśmy podstawowe informacje dotyczące mechanizmu kontroli optymistycznej współbieżności oraz sposób dodać go do kontrolki ObjectDataSource samouczka. W tym samouczku utworzymy retuszowanie na podstawowych mechanizmu kontroli optymistycznej współbieżności i następnie zobacz, jak wdrożyć ją za pomocą SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Podsumowanie optymistycznej współbieżności

Dla aplikacji sieci web, które pozwalają wielu równoczesnych użytkowników, aby edytować lub usunąć tych samych danych, istnieje możliwość, że jeden użytkownik przypadkowo może zastąpić inną zmiany s. W [Implementowanie optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) samouczek I podane w poniższym przykładzie:

Wyobraź sobie, że dwóch użytkowników, Jisun i Szymon, zostały zarówno odwiedzając strony w aplikacji, która może osoby odwiedzające aktualizowanie i usuwanie produkty za pomocą kontrolki GridView. Tym samym czasie, dla Chai zarówno kliknij przycisk Edytuj. Jisun zmienia nazwę produktu do herbaty Chai i klika przycisk Aktualizuj. Wynikiem jest `UPDATE` instrukcji, które są wysyłane do bazy danych, która ustawia *wszystkich* pól można aktualizować produkt s (mimo że Jisun aktualizowane tylko jedno pole `ProductName`). W tym momencie baza danych ma wartości Chai herbaty, kategorii Beverages, płynów egzotycznych dostawcy, i tak dalej dla tego konkretnego produktu. Jednak GridView na ekranie s Sam nadal zawiera nazwy produktu w można edytować wiersza w widoku GridView jako Chai. Kilka sekund po Jisun s zmiany zostały zatwierdzone, Sam aktualizacje kategorii Condiments i klika aktualizacji. Skutkuje to `UPDATE` instrukcji wysyłane do bazy danych, która ustawia nazwę produktu Chai, `CategoryID` do odpowiedniego Identyfikatora kategorii Condiments i tak dalej. Zostały zastąpione Jisun s zmiany nazwy produktu.

Na rysunku 2 przedstawiono ta interakcja.


[![Po dwóch użytkowników jednocześnie zaktualizowania rekordu, istnieje ryzyko s s jeden użytkownik zmieni się na zastąpić inne zasoby](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image1.png)

**Rysunek 2**: gdy dwóch użytkowników jednocześnie aktualizacji rekordu nie s potencjał dla jednego użytkownika s zmieni się na zastąpić inne zasoby ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image2.png))


Aby zapobiec w tym scenariuszu unfolding formę [kontroli współbieżności](http://en.wikipedia.org/wiki/Concurrency_control) musi zostać wdrożone. [Optymistyczna współbieżność](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) fokus w tym samouczku działa na założeniu, że może być konfliktów współbieżności i, wynikają większość czasu takie konflikty wygrał t. W związku z tym jeśli wystąpić konflikt, mechanizmu kontroli optymistycznej współbieżności po prostu informuje użytkownika, t może ich zmiany można zapisać, ponieważ inny użytkownik zmodyfikował tych samych danych.

> [!NOTE]
> W przypadku aplikacji, w którym zakłada się, że będzie istniało wiele konfliktów współbieżności, lub jeśli takie konflikty nie są dopuszczalna następnie mechanizm kontroli pesymistycznej współbieżności można zamiast tego. Odwołaj się do [Implementowanie optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) samouczek bardziej szczegółowe omówienie dotyczące kontroli pesymistycznej współbieżności.


Mechanizmu kontroli optymistycznej współbieżności działa przez zapewnienie im rekordu są zaktualizowane lub usunięte ma takie same wartości, tak jak podczas aktualizowania lub usuwania procesu uruchamiania. Na przykład po kliknięciu przycisku edycji w edycji kontrolki GridView wartości rekordu s są odczytu z bazy danych i wyświetlane w polach tekstowych i innych formantów sieci Web. Te oryginalne wartości są zapisywane w widoku GridView. Później, po użytkownik wprowadza swoje zmiany i kliknie przycisk Aktualizuj `UPDATE` instrukcją użytą w należy wziąć pod uwagę oryginalnych wartości, a także nowe wartości i aktualizować tylko podstawowy rekordu bazy danych, jeśli oryginalne wartości, że użytkownik rozpoczął edycję są identyczne do wartości w bazie danych. Rysunek 3 przedstawia następująca sekwencja zdarzeń.


[![Update lub Delete, które zakończyło się sukcesem oryginalne wartości, musi być równa wartości bieżącej bazy danych](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image3.png)

**Rysunek 3**: For Update lub Delete, aby odnieść sukces, oryginalnym wartości musi być równa wartości bieżącej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.png))


Istnieją różne metody Implementowanie optymistycznej współbieżności (zobacz [Peter A. Bromberg](http://www.eggheadcafe.com/articles/pbrombergresume.asp) s [Optmistic współbieżności aktualizowanie logiki](http://www.eggheadcafe.com/articles/20050719.asp) dla krótki przegląd szereg opcji). Rozszerza technikę, przy użyciu kontrolki SqlDataSource (a także ADO.NET wpisanych zestawów danych używanych w naszej warstwy dostępu do danych) `WHERE` klauzuli obejmujący porównanie wszystkich oryginalnych wartości. Następujące `UPDATE` instrukcji, na przykład aktualizuje nazwę i cena produktu tylko wtedy, gdy wartości bieżącej bazy danych są równe wartości, które zostały pierwotnie pobrany podczas aktualizowania rekordu w widoku GridView. `@ProductName` i `@UnitPrice` parametrów zawiera nowe wartości wprowadzonej przez użytkownika, natomiast `@original_ProductName` i `@original_UnitPrice` zawierają wartości, które zostały pierwotnie załadowane do kontrolki GridView kliknięcie przycisku Edytuj:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample1.sql)]

Jak opisano w tym samouczku, włączenie mechanizmu kontroli optymistycznej współbieżności przy użyciu kontrolki SqlDataSource jest tak proste, jak zaznaczenia pola wyboru.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Krok 1: Tworzenie SqlDataSource obsługującego optymistycznej współbieżności

Zacznij od otwarcia `OptimisticConcurrency.aspx` strony `SqlDataSource` folderu. Przeciągnij kontrolki SqlDataSource z przybornika w Projektancie ustawień jego `ID` właściwość `ProductsDataSourceWithOptimisticConcurrency`. Następnie kliknij łącze Konfigurowanie źródła danych za pomocą tagu inteligentnego sterowania s. Na pierwszym ekranie kreatora wybierz do pracy z `NORTHWINDConnectionString` i kliknij przycisk Dalej.


[![Wybierz do pracy z NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.png)

**Rysunek 4**: możliwość pracy z `NORTHWINDConnectionString` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.png))


W tym przykładzie będziemy dodawać GridView, która umożliwia użytkownikom edytowanie `Products` tabeli. W związku z Konfiguruj ekranu instrukcji Select, wybierz `Products` tabeli z listy rozwijanej i wybierz `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumn, jak pokazano na rysunku 5.


[![Z tabeli Produkty zwracają ProductID, ProductName, UnitPrice i nieobsługiwane kolumny](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.png)

**Rysunek 5**: Z `Products` tabeli, zwróć `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumn ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.png))


Po wybraniu kolumny, kliknij przycisk Zaawansowane, aby wyświetlić okno dialogowe Zaawansowane opcje generowania SQL. Sprawdź Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcji i użyj pól wyboru optymistycznej współbieżności i kliknij przycisk OK (odnoszą się do rysunku 1 dla zrzut ekranu). Ukończ pracę kreatora, klikając przycisk Dalej, a następnie Zakończ.

Po zakończeniu pracy kreatora Konfigurowanie źródła danych, Poświęć chwilę na zbadanie wynikowy `DeleteCommand` i `UpdateCommand` właściwości i `DeleteParameters` i `UpdateParameters` kolekcji. W tym celu najłatwiej kliknij w lewym dolnym rogu, aby wyświetlić stronę składni deklaratywnej s na karcie Źródło. Można znaleźć `UpdateCommand` wartość:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample2.sql)]

Z siedmiu parametrów w `UpdateParameters` kolekcji:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample3.aspx)]

Podobnie `DeleteCommand` właściwości i `DeleteParameters` kolekcji powinien wyglądać podobnie do poniższego:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample5.aspx)]

Oprócz rozszerzając `WHERE` klauzul `UpdateCommand` i `DeleteCommand` właściwości (i dodawanie dodatkowych parametrów do kolekcji odpowiednich parametrów), wybierając Użyj optymistycznej współbieżności opcja dostosowuje dwie inne Właściwości:

- Zmiany [ `ConflictDetection` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) z `OverwriteChanges` (ustawienie domyślne) do `CompareAllValues`
- Zmiany [ `OldValuesParameterFormatString` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) z {0} (ustawienie domyślne) do oryginalnego\_ {0} .

Gdy dane formantu sieci Web wywołuje SqlDataSource s `Update()` lub `Delete()` metody przekazuje w oryginalnych wartości. Jeśli SqlDataSource s `ConflictDetection` właściwość jest ustawiona na `CompareAllValues`, te oryginalne wartości są dodawane do polecenia. `OldValuesParameterFormatString` Dostarcza wzorzec nazewnictwa używane dla oryginalnej wartości parametrów. Kreator konfigurowania źródła danych przy użyciu oryginalnego\_ {0} oraz nazwy każdego parametru oryginalnego `UpdateCommand` i `DeleteCommand` właściwości i `UpdateParameters` i `DeleteParameters` kolekcji odpowiednio.

> [!NOTE]
> Ponieważ firma Microsoft re nie używa s kontrolki SqlDataSource Wstawianie możliwości, możesz usunąć `InsertCommand` właściwości i jego `InsertParameters` kolekcji.


## <a name="correctly-handlingnullvalues"></a>Obsługa poprawnie`NULL`wartości

Niestety, rozszerzone `UPDATE` i `DELETE` są automatycznie instrukcji wygenerowana przez Kreatora konfigurowania źródła danych, gdy optymistycznej współbieżności *nie* działają z rekordów, które zawierają `NULL` wartości. Aby poznać powody, należy wziąć pod uwagę nasz s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample6.sql)]

`UnitPrice` Kolumny w `Products` tabela może mieć `NULL` wartości. Jeśli określony rekord ma `NULL` wartość `UnitPrice`, `WHERE` część klauzuli `[UnitPrice] = @original_UnitPrice` będzie *zawsze* obliczyć wartość false, ponieważ `NULL = NULL` zawsze zwraca wartość False. W związku z tym, rekordy zawierające `NULL` wartości nie można edytować lub usuwać, jako `UPDATE` i `DELETE` instrukcji `WHERE` klauzule wygrał t zwrócenia wszystkich wierszy, aby zaktualizować lub usunąć.

> [!NOTE]
> Ta usterka najpierw zostało zgłoszone do firmy Microsoft w czerwcu 2004 r. w [SqlDataSource generuje niepoprawny instrukcji SQL](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) i zaplanowano powinno zostać rozwiązany w następnej wersji platformy ASP.NET.


Aby rozwiązać ten problem, będziemy musieli ręcznie aktualizować `WHERE` klauzule w obu `UpdateCommand` i `DeleteCommand` właściwości **wszystkich** kolumn, które mogą mieć `NULL` wartości. Ogólnie rzecz biorąc, zmień `[ColumnName] = @original_ColumnName` do:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample7.sql)]

Ta modyfikacja może się bezpośrednio za pośrednictwem oznaczeniu deklaracyjnym, za pośrednictwem opcji UpdateQuery lub DeleteQuery z okna właściwości lub aktualizacja i usuwanie kart w stronie Określ niestandardową instrukcję SQL lub procedury składowanej opcji Konfigurowanie danych Kreator źródła. Ponownie, należy przewidzieć modyfikacji *co* kolumny w `UpdateCommand` i `DeleteCommand` s `WHERE` klauzula, która może zawierać `NULL` wartości.

Powoduje zastosowanie do naszego przykładu następujących modyfikacji `UpdateCommand` i `DeleteCommand` wartości:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Krok 2: Dodawanie GridView z funkcją Edytuj i opcje usuwania

Dzięki użyciu kontrolki SqlDataSource skonfigurowany do obsługi optymistycznej współbieżności pozostaje można dodać danych formantu sieci Web do strony, która korzysta z tej kontroli współbieżności. W tym samouczku umożliwiają s Dodaj GridView zapewniająca zarówno edycji oraz funkcję usuwania. Aby to zrobić, przeciągnij GridView z przybornika do projektanta i ustaw jego `ID` do `Products`. Za pomocą tagu inteligentnego s GridView powiązać `ProductsDataSourceWithOptimisticConcurrency` kontrolki SqlDataSource dodanego w kroku 1. Na koniec sprawdź opcje Włącz edytowanie i usuwanie włączyć za pomocą tagu inteligentnego.


[![Powiąż widoku GridView z kontrolką SqlDataSource i Włącz edytowanie i usuwanie](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.png)

**Rysunek 6**: powiązywanie kontrolki GridView SqlDataSource i Włącz edytowanie i usuwanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image10.png))


Po dodaniu kontrolki GridView, skonfiguruj jego wygląd poprzez usunięcie `ProductID` elementu BoundField, zmieniając `ProductName` s elementu BoundField `HeaderText` właściwości produktu i aktualizowanie `UnitPrice` elementu BoundField tak, aby jego `HeaderText` właściwość po prostu cena. W idealnym przypadku d zwiększania interfejs edytowania obejmujący RequiredFieldValidator dla `ProductName` wartość i CompareValidator dla `UnitPrice` wartość (tak, aby upewnić się, że prawidłowo sformatowaną wartość liczbową s). Zapoznaj się [Dostosowywanie interfejsu modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) samouczek dotyczący bardziej przyjrzeć się Dostosowywanie s GridView edytowanie interfejsu.

> [!NOTE]
> GridView, można włączyć stan widoku s, ponieważ oryginalne wartości, które są przekazywane z kontrolki GridView do SqlDataSource są przechowywane w widoku stanu.


Po wprowadzeniu tych zmian do kontrolki GridView, oznaczeniu deklaracyjnym kontrolkami GridView i użyciu kontrolki SqlDataSource powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample9.aspx)]

Aby wyświetlić kontroli optymistycznej współbieżności w akcji, Otwórz dwa okna przeglądarki, a następnie załadować `OptimisticConcurrency.aspx` strony w obu. Kliknij przyciski Edytuj pierwszy produkt w obu przeglądarkach. W jednej przeglądarki Zmień nazwę produktu, a następnie kliknij przycisk aktualizacji. Przeglądarka będzie ogłaszanie zwrotne i widoku GridView powróci do trybu edycji wstępnie przedstawiający nową nazwę produktu dla rekordu, po prostu edytować.

W drugim oknie przeglądarki Zmień ceny (ale pozostaw nazwę produktu jako początkowej wartości), a następnie kliknij przycisk Aktualizuj. Na odświeżenie strony siatki powraca do trybu edycji wstępnie, ale zmiana ceny nie została zarejestrowana. Drugi przeglądarkę taką samą wartość jak pierwszy z nich Nowa nazwa jest wyświetlana produktu za pomocą stara cena. Zmiany wprowadzone w drugim oknie przeglądarki zostały utracone. Ponadto zmiany zostały utracone zamiast ciche, ponieważ wystąpił bez wyjątku i komunikat informujący o naruszenie współbieżności właśnie wykonana.


[![Zmiany w drugim oknie przeglądarki zostały utracone w trybie dyskretnym](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image11.png)

**Rysunek 7**: zmiany w drugim przeglądarki okna zostały dyskretnie utraty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image12.png))


Był powód, dlaczego drugi przeglądarki s zmiany nie zostały zatwierdzone, ponieważ `UPDATE` instrukcja s `WHERE` klauzula odfiltrowane wszystkie rekordy i w związku z tym, nie wpływa na wszystkich wierszy. Pozwól s Przyjrzyj się `UPDATE` instrukcję ponownie:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample10.sql)]

Po drugie okno przeglądarki aktualizuje rekord, oryginalna nazwa produktu określone w `WHERE` klauzuli są zgodne się przy użyciu istniejącej nazwy produktu (ponieważ został on zmieniony przez pierwsze przeglądarki). W związku z tym, instrukcja `[ProductName] = @original_ProductName` zwraca wartość False, a `UPDATE` nie ma wpływu na wszystkie rekordy.

> [!NOTE]
> Usuń działa w taki sam sposób. Za pomocą dwóch okna przeglądarki otwartego Rozpocznij od edycji danego produktu przy użyciu jednego, a następnie zapisanie jej zmiany. Po zapisaniu zmian w jednej przeglądarki, kliknij przycisk Usuń dla tego samego produktu w innym. Ponieważ oryginalne don wartości t dopasować w `DELETE` instrukcja s `WHERE` klauzuli delete dyskretnie nie powiedzie się.


Z perspektywy użytkownika końcowego s w drugim oknie przeglądarki po kliknięciu przycisku Aktualizuj siatki powraca do trybu edycji wstępnie, ale ich zmiany zostały utracone. Jednak miejsca s nie wizualną opinię, która trzymaj t uaktualnili swoje zmiany. Najlepiej Jeśli zmiany użytkownika s zostaną utracone na naruszenie współbieżności, możemy d powiadamiać użytkowników i, Zachowaj siatki w trybie edycji. Pozwól, s, zobacz, jak to zrobić.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Krok 3: Określanie, kiedy nastąpiło naruszenie współbieżności

Ponieważ Naruszenie współbieżności odrzuca zmiany, które wprowadził jeden, jest dobre rozwiązanie ostrzec użytkownika, jeśli nastąpiło naruszenie współbieżności. Aby ostrzec użytkownika, umożliwiają s dodać formant etykiety w sieci Web do górnej części strony o nazwie `ConcurrencyViolationMessage` którego `Text` właściwości wyświetla następujący komunikat: Podjęto próbę aktualizacji lub usunięcia rekordu, który jednocześnie został zaktualizowany przez innego użytkownika. . Przejrzyj zmiany wprowadzone przez użytkownika a następnie wykonaj ponownie aktualizacji lub usunięcia. Ustaw formant etykiety s `CssClass` właściwość ostrzeżenie, czyli klasę CSS zdefiniowanych w `Styles.css` który wyświetla tekst czcionką czerwony, kursywy, pogrubiony i dużych. Wreszcie, ustaw właściwość etykiety s `Visible` i `EnableViewState` właściwości `false`. To spowoduje ukrycie etykiety z wyjątkiem tylko ogłaszania, te zwrotnego gdzie możemy jawnie ustawić jej `Visible` właściwość `true`.


[![Dodaj kontrolkę typu etykieta do strony, aby wyświetlić ostrzeżenia](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image13.png)

**Rysunek 8**: Dodaj kontrolkę typu etykieta do strony, aby wyświetlić ostrzeżenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image14.png))


Podczas przeprowadzania aktualizacji lub usuwania, GridView s `RowUpdated` i `RowDeleted` procedury obsługi zdarzeń fire po pomyślnym zakończeniu kontroli źródła danych ma żądanego update lub delete. Można określić liczbę wierszy objętych operacji z tych programów obsługi zdarzeń. Jeśli zero wierszy została zmieniona, chcemy wyświetlić `ConcurrencyViolationMessage` etykiety.

Utwórz procedurę obsługi zdarzeń dla obu `RowUpdated` i `RowDeleted` zdarzeń i Dodaj następujący kod:


[!code-csharp[Main](implementing-optimistic-concurrency-with-the-sqldatasource-cs/samples/sample11.cs)]

W obu procedurach obsługi zdarzeń sprawdzenie `e.AffectedRows` właściwości i, jeśli jest ona równa 0, ustaw `ConcurrencyViolationMessage` etykiety s `Visible` właściwość `true`. W `RowUpdated` procedura obsługi zdarzeń, możemy również poinstruować GridView pozostanie w trybie edycji przez ustawienie `KeepInEditMode` właściwości na wartość true. W ten sposób, musimy ponownie powiązać dane do siatki, tak aby innych danych użytkowników s jest ładowany do interfejsu edycji. Jest to realizowane przez wywołanie metody GridView s `DataBind()` metody.

Zgodnie z rysunku nr 9 przedstawiono z tych dwóch zdarzenia, bardzo istotne wyświetlany jest komunikat przy każdym wystąpieniu Naruszenie współbieżności.


[![Zostanie wyświetlony komunikat w przypadku naruszenia współbieżności](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image15.png)

**Rysunek 9**: komunikat jest wyświetlany w przypadku naruszenia współbieżności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-cs/_static/image16.png))


## <a name="summary"></a>Podsumowanie

Podczas tworzenia aplikacji sieci web gdzie wielu równoczesnych użytkowników może edytować tych samych danych, należy rozważyć opcje kontroli współbieżności. Domyślnie dane programu ASP.NET, które kontrolki sieci Web i kontrolki źródła danych nie używają żadnych kontroli współbieżności. Jak firma Microsoft jest opisany w tym samouczku, implementacja mechanizmu kontroli optymistycznej współbieżności przy użyciu kontrolki SqlDataSource jest stosunkowo szybko i łatwo. SqlDataSource obsługuje większość żmudne zadania dla Twojego Dodawanie rozszerzonych `WHERE` klauzul, aby automatycznie wygenerowany `UPDATE` i `DELETE` instrukcji, ale jest kilka precyzyjnie w obsłudze `NULL` wartości kolumn, zgodnie z opisem w Obsługa poprawnie `NULL` wartości sekcji.

Nasze badania SqlDataSource kończy się w tym samouczku. Naszych pozostałych samouczków powróci do pracy z danymi za pomocą kontrolki ObjectDataSource i architektury warstwowej.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
> [dalej](querying-data-with-the-sqldatasource-control-vb.md)
