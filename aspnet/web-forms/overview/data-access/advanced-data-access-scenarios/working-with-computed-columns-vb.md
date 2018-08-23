---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Praca z kolumnami obliczanymi (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Po utworzeniu tabeli bazy danych programu Microsoft SQL Server pozwala na zdefiniowanie kolumną obliczaną, którego wartość jest obliczana na podstawie wyrażenia to zwykle referen...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 81254408252c5e786c938d4eb8beb1c7a2b65218
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754377"
---
<a name="working-with-computed-columns-vb"></a>Praca z kolumnami obliczanymi (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) lub [Pobierz plik PDF](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Po utworzeniu tabeli bazy danych programu Microsoft SQL Server umożliwia definiowanie kolumną obliczaną, którego wartość jest obliczana z wyrażeniem odwołującym inne wartości w jednym rekordzie bazy danych. Wartości te są tylko do odczytu w bazy danych, która wymaga szczególnej uwagi, pracując z TableAdapters. W tym samouczku będziemy Dowiedz się, jak sprostać wyzwaniom związanego z kolumnami obliczanymi.


## <a name="introduction"></a>Wprowadzenie

Microsoft SQL Server umożliwia  *[kolumn obliczanych](https://msdn.microsoft.com/library/ms191250.aspx)*, które są kolumn, których wartości są obliczane na podstawie wyrażenie, które zazwyczaj odwołuje się do wartości z innych kolumn w tej samej tabeli. Na przykład raz śledzenia modelu danych może mieć tabelę o nazwie `ServiceLog` z kolumnami, w tym `ServicePerformed`, `EmployeeID`, `Rate`, i `Duration`, między innymi. Podczas gdy wynosi należna kwota na usługę elementu (współczynnik pomnożonej przez czas trwania) może być obliczany za pośrednictwem strony sieci web lub innych interfejsu programowego, może być przydatna, aby zawierała kolumnę w `ServiceLog` tabelę o nazwie `AmountDue` , to zgłoszone informacje. W tej kolumnie mógł zostać utworzony jako normalny kolumny, ale go musi zostać zaktualizowany w dowolnym momencie `Rate` lub `Duration` zmianie wartości w kolumnie. Lepszym rozwiązaniem byłoby zapewnienie `AmountDue` kolumny przy użyciu wyrażenia kolumny obliczanej `Rate * Duration`. To spowodowałoby programu SQL Server automatycznie obliczyć `AmountDue` wartości w kolumnie zawsze wtedy, gdy do niej odwoływano w zapytaniu.

Ponieważ wartość kolumny obliczanej s jest określana przez wyrażenie, takich kolumn są przeznaczone tylko do odczytu i w związku z tym nie można przypisać wartość do nich w `INSERT` lub `UPDATE` instrukcji. Jednak gdy kolumny obliczane są częścią główne zapytanie do TableAdapter, która używa instrukcji SQL zapytań ad-hoc, są one automatycznie uwzględnione w generowanych automatycznie `INSERT` i `UPDATE` instrukcji. W związku z tym, s TableAdapter `INSERT` i `UPDATE` zapytań i `InsertCommand` i `UpdateCommand` można zaktualizować właściwości, aby usunąć odwołania do żadnej kolumny obliczanej.

Jednym z wyzwań przy użyciu obliczonych kolumn za pomocą TableAdapter, która używa instrukcji SQL zapytań ad-hoc jest to, że TableAdapter s `INSERT` i `UPDATE` zapytania są automatycznie generowane każdym zakończeniu Kreator konfiguracji TableAdapter. W związku z tym, kolumnach obliczanych ręcznie usunięty z `INSERT` i `UPDATE` zapytań pojawi się ponownie, jeśli Kreator jest uruchamiać ponownie. Don TableAdapters, których używanie procedur składowanych t borykają się z tym kruchości, mają własne Osobliwości, które będzie można rozwiązać w kroku 3.

W tym samouczku zostanie dodany do kolumny obliczanej `Suppliers` tabeli w bazie danych Northwind, a następnie utwórz odpowiedni obiekt TableAdapter do pracy z tej tabeli i jej kolumny obliczanej. Mamy nasz TableAdapter używanie procedur składowanych zamiast instrukcji SQL zapytań ad-hoc, dzięki czemu nasze t są dostosowania utracone podczas służy Kreator konfiguracji TableAdapter.

Rozpocznij pracę dzięki s!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Krok 1: Dodawanie kolumny obliczanej do`Suppliers`tabeli

Bazy danych Northwind nie ma żadnych kolumn obliczanych, dlatego firma Microsoft będzie należy dodać jeden określić główną przyczynę. Dla tego samouczka umożliwiają s Dodaj kolumnę obliczaną do `Suppliers` tabeli o nazwie `FullContactName` zwracającego nazwę kontaktu s, tytułu i firmy pracują w następującym formacie: `ContactName` (`ContactTitle`, `CompanyName`). To obliczanej kolumny mogą być używane w raportach, podczas wyświetlania informacji o dostawcy.

Zacznij od otwarcia `Suppliers` definicja tabeli, klikając prawym przyciskiem myszy `Suppliers` tabeli w Eksploratorze serwera i wybierając polecenie Otwórz definicję tabeli, z menu kontekstowego. Spowoduje to wyświetlenie kolumn w tabeli i ich właściwości, takie jak ich typ danych, czy umożliwiają one `NULL` s i tak dalej. Aby dodać kolumnę obliczaną, uruchom przez wpisanie nazwy kolumny w definicji tabeli. Następnie wprowadź jego wyrażenie w polu tekstowym (Formuła) w sekcji obliczona Specyfikacja kolumny w oknie dialogowym właściwości kolumny (patrz rysunek 1). Nazwa kolumny obliczanej `FullContactName` i użyj następującego wyrażenia:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Należy pamiętać, że ciągi mogą być łączone w języku SQL przy użyciu `+` operatora. `CASE` Poufności informacji mogą być używane jak warunkowe w tradycyjnym języku programowania. W powyższym wyrażeniu `CASE` instrukcja może zostać odczytany jako: Jeśli `ContactTitle` nie `NULL` następnie wyprowadzić `ContactTitle` połączonych za pomocą przecinka, w przeciwnym razie wartość emisji, nic nie. Aby uzyskać więcej informacji na temat przydatność `CASE` instrukcji, zobacz [Power SQL `CASE` instrukcji](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Zamiast używania `CASE` instrukcji w tym miejscu można też użyliśmy `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Zwraca *checkExpression* Jeśli jest inna niż NULL, w przeciwnym razie zwraca *replacementValue*. Podczas gdy albo `ISNULL` lub `CASE` będzie działać w tym wypadku są bardziej skomplikowanych scenariuszach gdzie elastyczność `CASE` instrukcji nie można dopasować do podwyrażenia `ISNULL`.


Po dodaniu tę kolumnę obliczaną ekran powinien wyglądać jak ekran zrzut na rysunku 1.


[![Dodaj kolumnę obliczaną, o nazwie FullContactName do tabeli dostawcy](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Rysunek 1**: Dodaj obliczane kolumny o nazwie `FullContactName` do `Suppliers` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image3.png))


Po nazewnictwa kolumna obliczana i wprowadzanie jej wyrażenia, Zapisz zmiany w tabeli, klikając ikonę Zapisz na pasku narzędzi, naciskając klawisze Ctrl + S lub przechodząc do menu Plik i wybierając polecenie Zapisz `Suppliers`.

Zapisywanie tabeli należy odświeżyć Eksploratora serwera, po prostu dodać kolumny w tym `Suppliers` listy kolumn tabeli s. Ponadto wyrażenie wprowadzony w polu tekstowym (formuły) spowoduje automatyczne dopasowanie równoważne wyrażenie, które usuwa niepotrzebnych odstępów, otacza nazwy kolumn w nawiasach kwadratowych (`[]`) i zawiera nawiasy, aby wyraźniej pokazać kolejność operacji:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Aby uzyskać więcej informacji na temat kolumn obliczanych w programie Microsoft SQL Server, zobacz [dokumentacji technicznej](https://msdn.microsoft.com/library/ms191250.aspx). Również szybko sprawdzić wydajność [porady: Określanie kolumn obliczonych](https://msdn.microsoft.com/library/ms188300.aspx) przewodnik krok po kroku tworzenia kolumn obliczanych.

> [!NOTE]
> Domyślnie kolumnach obliczanych fizycznie nie są przechowywane w tabeli, ale zamiast tego są ponownie obliczane każdorazowo, gdy występuje do nich w zapytaniu. Zaznaczając pole wyboru jest trwały, jednak można poinstruować program SQL Server do fizycznie przechowywania kolumny obliczanej w tabeli. Dzięki temu indeks ma zostać utworzony w kolumnie obliczanej, co może poprawić wydajność zapytań, które używają wartości kolumny obliczanej w ich `WHERE` klauzul. Zobacz [tworzenie indeksów dla kolumny obliczane](https://msdn.microsoft.com/library/ms189292.aspx) Aby uzyskać więcej informacji.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Krok 2: Wyświetlanie kolumny obliczanej wartości s

Zanim zaczniemy pracę w warstwie dostępu do danych umożliwiają s potrwać chwilę, aby wyświetlić `FullContactName` wartości. Z poziomu Eksploratora serwera, kliknij prawym przyciskiem myszy `Suppliers` nazwę tabeli i z menu kontekstowego wybierz opcję nowe zapytanie. Spowoduje to wyświetlenie okna zapytania, które wyświetla monit o nam wybrać tabele do uwzględnienia w zapytaniu. Dodaj `Suppliers` tabeli i kliknij przycisk Zamknij. Następnie sprawdź `CompanyName`, `ContactName`, `ContactTitle`, i `FullContactName` kolumny z tabeli dostawcy. Na koniec kliknij ikonę czerwonego wykrzyknika, na pasku narzędzi, aby wykonać zapytanie i wyświetlić wyniki.

Jak pokazano na rysunku 2, wyniki obejmują `FullContactName`, które zawiera listę `CompanyName`, `ContactName`, i `ContactTitle` kolumn przy użyciu formatu `ContactName` (`ContactTitle`, `CompanyName`).


[![FullContactName używa formatu ContactName (StanowiskoPrzedstawiciela, nazwa firmy)](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Rysunek 2**: `FullContactName` używa formatu `ContactName` (`ContactTitle`, `CompanyName`) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Krok 3. Dodawanie`SuppliersTableAdapter`do warstwy dostępu do danych

Aby pracować z informacji o dostawcy w naszej aplikacji należy najpierw utworzyć obiekt TableAdapter i DataTable w naszym DAL. W idealnym przypadku to będzie można osiągnąć proste kroki badania w samouczkach wcześniej. Jednak praca z kolumnami obliczanymi wprowadza kilka zagnieceń, które że opiszemy je w dyskusji.

Jeśli używasz TableAdapter, która używa instrukcji SQL zapytań ad-hoc, po prostu można uwzględnić kolumny obliczanej w TableAdapter s główne zapytanie za pomocą Kreatora konfiguracji TableAdapter. W efekcie jednak generowane automatycznie `INSERT` i `UPDATE` instrukcji, które obejmują kolumny obliczanej. Jeśli spróbujesz wykonać jedną z następujących metod `SqlException` komunikatem kolumny *ColumnName* nie można zmodyfikować, ponieważ jest to kolumna obliczana albo lub znajduje się wynik operatora UNION, zostanie zgłoszony. Gdy `INSERT` i `UPDATE` instrukcji można ręcznie dostosować za pomocą TableAdapter s `InsertCommand` i `UpdateCommand` właściwości, te dostosowania zostaną utracone zawsze wtedy, gdy Kreator konfiguracji TableAdapter uruchamiać ponownie.

Z powodu kruchości TableAdapters, użyj instrukcji SQL zapytań ad-hoc zalecane jest, gdy praca z kolumnami obliczanymi możemy używanie procedur składowanych. Jeśli korzystasz z istniejącymi procedurami składowanymi, po prostu skonfigurować TableAdapter zgodnie z opisem w [za pomocą istniejących procedur składowanych dla s wpisany zestaw danych TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) samouczka. Jeśli Kreator TableAdapter Tworzenie procedur składowanych dla Ciebie, jednak ważne jest pominąć początkowo żadnych kolumn obliczanych z głównego zapytania. Jeśli dołączysz kolumny obliczanej w głównym zapytaniu Kreator konfiguracji TableAdapter poinformuje, po jego ukończeniu, nie może utworzyć odpowiednich procedur składowanych. Krótko mówiąc, należy wstępnie skonfigurować TableAdapter za pomocą obliczanej kolumny bez główne zapytanie, a następnie ręcznie zaktualizować odpowiednich procedur składowanych i TableAdapter s `SelectCommand` aby zawierała kolumnę obliczaną. Ta metoda jest podobna do komentarzowi użytemu w [aktualizowanie elementu TableAdapter w celu używania](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* samouczka.

Na potrzeby tego samouczka należy zezwolić s, Dodaj nowy obiekt TableAdapter i jego automatyczne tworzenie procedur składowanych dla nas. W związku z tym, konieczne będzie początkowo pominąć `FullContactName` kolumny obliczanej z głównego zapytania.

Zacznij od otwarcia `NorthwindWithSprocs` zestawu danych w `~/App_Code/DAL` folderu. Kliknij prawym przyciskiem myszy w projektancie, a następnie z menu kontekstowego wybierz dodać nowy obiekt TableAdapter. Spowoduje to uruchomienie Kreatora konfiguracji TableAdapter. Określ bazę danych do zapytania o dane z (`NORTHWNDConnectionString` z `Web.config`) i kliknij przycisk Dalej. Ponieważ mamy jeszcze nie utworzono żadnych procedur składowanych do wykonywania zapytań i modyfikowania `Suppliers` tabeli, wybierz pozycję Utwórz nowe procedury składowane opcję Tak, aby Kreator utworzone dla nas i kliknij przycisk Dalej.


[![Wybierz pozycję Utwórz nowe procedury składowane opcji](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Rysunek 3**: wybierz pozycję Utwórz nowe procedury składowane opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image9.png))


Kolejny krok nam monituje o podanie główne zapytanie. Wprowadź następujące zapytanie zwraca `SupplierID`, `CompanyName`, `ContactName`, i `ContactTitle` kolumny dla każdego dostawcy. Należy pamiętać, że to zapytanie celowo pomija kolumny obliczanej (`FullContactName`); firma Microsoft aktualizuje odpowiedni procedurę przechowywaną, aby zawierało tej kolumny w kroku 4.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Po wprowadzeniu główne zapytanie i klikając przycisk Dalej, Kreator pozwala nam nazwy czterech procedur składowanych, który zostanie wygenerowany. Nazwy tych procedurach składowanych `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, i `Suppliers_Delete`, tak jak pokazano na rysunku 4.


[![Dostosowywanie nazw automatycznego generowania procedur składowanych](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Rysunek 4**: Dostosowywanie nazw procedur składowanych wygenerowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image12.png))


Następny krok kreatora pozwala nam nazwy metody TableAdapter s i określ wzorce umożliwiają dostęp i aktualizują dane. Pozostaw zaznaczone wszystkie trzy pola wyboru, ale Zmień nazwę `GetData` metody `GetSuppliers`. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.


[![Zmień nazwę metody GetData GetSuppliers](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Rysunek 5**: zmiana nazwy `GetData` metody `GetSuppliers` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image15.png))


Po kliknięciu Zakończ, kreator Utwórz czterech procedur składowanych i dodać TableAdapter i odpowiedniego elementu DataTable do wpisana zestawu danych.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Krok 4: W tym kolumny obliczanej w głównym zapytaniu s TableAdapter

Teraz musimy zaktualizować TableAdapter i DataTable utworzonego w kroku 3, aby uwzględnić `FullContactName` kolumny obliczanej. Ten proces obejmuje dwa kroki:

1. Aktualizowanie `Suppliers_Select` przechowywane procedury, aby zwrócić `FullContactName` kolumnę obliczaną, a
2. Aktualizowanie elementu DataTable do uwzględnienia odpowiednią `FullContactName` kolumny.

Rozpocznij, przechodząc do Eksploratora serwera i przechodzenie do szczegółów w folderze procedur składowanych. Otwórz `Suppliers_Select` przechowywane procedury i zaktualizuj `SELECT` zapytanie, aby uwzględnić `FullContactName` kolumny obliczanej:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Zapisz zmiany do procedury składowanej, klikając ikonę Zapisz na pasku narzędzi, naciskając klawisze Ctrl + S lub wybierając zapisywania `Suppliers_Select` opcję z menu Plik.

Następnie wróć do Projektanta obiektów DataSet, kliknij prawym przyciskiem myszy `SuppliersTableAdapter`i wybierz pozycję z menu kontekstowego. Należy pamiętać, że `Suppliers_Select` zawiera teraz kolumny `FullContactName` kolumny w jego kolekcja kolumn danych.


[![Uruchom Kreatora konfiguracji TableAdapter s, można zaktualizować kolumn s DataTable](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Rysunek 6**: uruchom s TableAdapter Kreator konfiguracji można zaktualizować elementu DataTable s kolumn ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image18.png))


Kliknij przycisk Zakończ, aby zakończyć działanie kreatora. To spowoduje automatyczne dodanie odpowiadającej mu kolumny do `SuppliersDataTable`. Kreator Obiekt TableAdapter jest inteligentnego wykryć, że `FullContactName` kolumna jest kolumną obliczaną, a więc tylko do odczytu. W związku z tym, ustawia kolumny s `ReadOnly` właściwość `true`. Aby to sprawdzić, wybierz kolumnę z `SuppliersDataTable` , a następnie przejdź do okna właściwości (zobacz rysunek 7). Należy pamiętać, że `FullContactName` kolumny s `DataType` i `MaxLength` właściwości również są odpowiednio ustawiane.


[![Kolumna FullContactName została oznaczona jako tylko do odczytu](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Rysunek 7**: `FullContactName` kolumna została oznaczona jako tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Krok 5. Dodawanie`GetSupplierBySupplierID`metody TableAdapter

W tym samouczku zostanie utworzona strona ASP.NET, która wyświetla dostawców w siatce można aktualizować. W przeszłości samouczki Zaktualizowaliśmy pojedynczy rekord z warstwy logiki biznesowej przez pobranie, czy określony rekord z warstwy DAL jako silnie typizowane DataTable, aktualizowanie jego właściwości, a następnie wysyłając zaktualizowane DataTable z powrotem do warstwy DAL propagowanie zmian Baza danych. Aby wykonać ten pierwszy krok — Pobieranie rekordu aktualizowana z warstwy DAL - należy najpierw dodać `GetSupplierBySupplierID(supplierID)` metody z warstwą dal.

Kliknij prawym przyciskiem myszy `SuppliersTableAdapter` w projekcie zestawu danych i wybierz opcję Dodaj zapytanie z menu kontekstowego. Ile My mieliśmy w kroku 3 umożliwiają kreatora wygenerować nową procedurę składowaną dla nas, wybierając opcję tworzenia nowej procedury składowanej (odnoszą się do rysunku 3 dla tego kroku w Kreatorze zrzut ekranu). Ponieważ ta metoda zwraca rekord z wielu kolumn, należy wskazać, że chcemy korzystać z zapytania SQL, który jest SELECT, która zwraca wiersze, a następnie kliknij przycisk Dalej.


[![Wybierz pozycję Wybierz, która zwraca wiersze — opcja](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Rysunek 8**: wybierz pozycję Wybierz, która zwraca wiersze opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image24.png))


Kolejny krok monituje o nas dla zapytania, aby użyć tej metody. Wprowadź następujące polecenie, które zwraca te same pola danych jako główne zapytanie, ale dla określonego dostawcy.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

Następny ekran pyta, czy nam Nazwa procedury składowanej, który ma być generowane automatycznie. Nazwa tej procedury składowanej `Suppliers_SelectBySupplierID` i kliknij przycisk Dalej.


[![Nazwa Suppliers_SelectBySupplierID procedury składowanej](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Rysunek 9**: Nazwa procedury składowanej `Suppliers_SelectBySupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image27.png))


Na koniec z poleceniami kreatora nam dane dostępu wzorców i nazwy metod na potrzeby TableAdapter. Pozostaw oba pola wyboru zaznaczone, ale Zmień nazwę `FillBy` i `GetDataBy` metody `FillBySupplierID` i `GetSupplierBySupplierID`, odpowiednio.


[![Nazwa FillBySupplierID metod TableAdapter i GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Na rysunku nr 10**: Nazwa metody TableAdapter `FillBySupplierID` i `GetSupplierBySupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image30.png))


Kliknij przycisk Zakończ, aby zakończyć działanie kreatora.

## <a name="step-6-creating-the-business-logic-layer"></a>Krok 6. Tworzenie warstwy logiki biznesowej

Przed utworzeniem strony ASP.NET, która używa kolumny obliczanej, utworzony w kroku 1, najpierw należy dodać odpowiednie metody w LOGIKI. Strony ASP.NET, która zostanie utworzona w kroku 7, pozwoli użytkownikom na wyświetlanie i edytowanie dostawcy. Dlatego musimy naszych LOGIKI, aby zapewnili co najmniej metodę, aby pobrać wszystkich dostawców i innej aktualizacji danego dostawcy.

Utwórz nowy plik klasy o nazwie `SuppliersBLLWithSprocs` w `~/App_Code/BLL` folderze i Dodaj następujący kod:


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Inne klasy LOGIKI, takich jak `SuppliersBLLWithSprocs` ma `Protected` `Adapter` właściwość, która zwraca wystąpienie `SuppliersTableAdapter` klasy wraz z dwóch `Public` metody: `GetSuppliers` i `UpdateSupplier`. `GetSuppliers` Wywołuje metodę i zwraca `SuppliersDataTable` zwrócony przez odpowiedni `GetSupplier` metody w warstwie dostępu do danych. `UpdateSupplier` Metoda pobiera informacje o dostawcy określonego aktualizowana przez wywołanie DAL s `GetSupplierBySupplierID(supplierID)` metody. Następnie aktualizuje `CategoryName`, `ContactName`, i `ContactTitle` właściwości i zatwierdza zmiany w bazie danych przez wywołanie metody s warstwy dostępu do danych `Update` metody, przekazując zmodyfikowanego `SuppliersRow` obiektu.

> [!NOTE]
> Z wyjątkiem `SupplierID` i `CompanyName`, Zezwalaj na wszystkie kolumny w tabeli dostawcy `NULL` wartości. W związku z tym jeśli przekazany do `contactName` lub `contactTitle` parametry są `Nothing` potrzebujemy zestawu, odpowiednie `ContactName` i `ContactTitle` właściwości `NULL` bazy danych przy użyciu wartości `SetContactNameNull` i `SetContactTitleNull`metod, odpowiednio.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Krok 7: Praca z kolumną obliczaną z warstwy prezentacji

Z kolumną obliczaną, dodane do `Suppliers` tabeli i warstwy DAL i LOGIKI odpowiednio aktualizowane, możemy przystąpić do zbudowania strony ASP.NET, która współdziała z `FullContactName` kolumny obliczanej. Zacznij od otwarcia `ComputedColumns.aspx` stronie `AdvancedDAL` folder i przeciągnij GridView z przybornika do projektanta. Ustaw GridView s `ID` właściwości `Suppliers` i z jego tag inteligentny powiązać go do nowego elementu ObjectDataSource, o nazwie `SuppliersDataSource`. Konfigurowanie kontrolki ObjectDataSource używać `SuppliersBLLWithSprocs` klasy dodaliśmy z powrotem w kroku 6 i kliknij przycisk Dalej.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy SuppliersBLLWithSprocs](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Rysunek 11**: Konfigurowanie kontrolki ObjectDataSource do użycia `SuppliersBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image33.png))


Dostępne są tylko dwie metody zdefiniowane w `SuppliersBLLWithSprocs` klasy: `GetSuppliers` i `UpdateSupplier`. Upewnij się, że te dwie metody są określone w polu Wybierz odpowiednio zaktualizować karty i kliknij przycisk Zakończ, aby zakończyć konfigurowanie kontrolki ObjectDataSource.

Po zakończeniu działania Kreatora konfiguracji źródła danych program Visual Studio spowoduje dodanie elementu BoundField dla każdego pola danych zwracane. Usuń `SupplierID` elementu BoundField i zmień `HeaderText` właściwości `CompanyName`, `ContactName`, `ContactTitle`, i `FullContactName` BoundFields do firmy, skontaktuj się z nazwy, tytułu i imię i nazwisko kontaktu, odpowiednio. Za pomocą tagu inteligentnego zaznacz pole wyboru Włącz edytowanie, aby włączyć kontrolki GridView s wbudowaną możliwość edycji.

Oprócz dodawania BoundFields do kontrolki GridView, uzupełnianie w Kreatorze źródła danych również powoduje, że Visual Studio, aby ustawić ObjectDataSource s `OldValuesParameterFormatString` właściwości z oryginalną\_{0}. Przywróć ustawienie powrót do jego wartość domyślną {0} .

Po wprowadzeniu tych zmian do kontrolki GridView i kontrolki ObjectDataSource, ich oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Następnie odwiedź tę stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 12, każdy dostawca znajduje się w siatce, która obejmuje `FullContactName` kolumny, której wartość jest po prostu łączenie trzy kolumny sformatowane jako `ContactName` (`ContactTitle`, `CompanyName`).


[![Każdy dostawca znajduje się w siatce](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Rysunek 12**: każdy dostawca znajduje się w siatce ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image36.png))


Klikając przycisk Edytuj, dla określonego dostawcę powoduje odświeżenie strony i ma ten wiersz, renderowane przy użyciu jego edytowania interfejsu (zobacz rysunek 13). Pierwsze trzy kolumny renderowane w swojej domyślnej edycji interfejsu — formant pola tekstowego, którego `Text` właściwość jest ustawiona na wartość pola danych. `FullContactName` Kolumny, jednak pozostaje jako tekst. Gdy BoundFields zostały dodane do kontrolki GridView po ukończeniu działania Kreatora konfiguracji źródła danych, `FullContactName` s elementu BoundField `ReadOnly` właściwość `True` ponieważ odpowiednich `FullContactName` kolumny w `SuppliersDataTable` ma jego `ReadOnly` właściwością `True`. Jak wspomniano w kroku 4 `FullContactName` s `ReadOnly` właściwość `True` ponieważ TableAdapter wykrył, że kolumny jest kolumną obliczaną.


[![Kolumna FullContactName jest niedostępne do edycji](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Rysunek 13**: `FullContactName` kolumna jest nie można edytować ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image39.png))


Przejdź dalej i zaktualizować wartość jednego lub więcej kolumn, które można edytować i kliknij przycisk Aktualizuj. Uwaga jak `FullContactName` s wartość jest automatycznie aktualizowana w celu odzwierciedlenia zmiany.

> [!NOTE]
> Kontrolki GridView aktualnie używa BoundFields dla edytowalnego pola skutkuje domyślny interfejs do edycji. Ponieważ `CompanyName` pole jest wymagane, powinny być konwertowane do TemplateField, która obejmuje RequiredFieldValidator. Można pozostawić to w charakterze ćwiczenia zainteresowanych czytnika. Zapoznaj się z [Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) samouczka, aby uzyskać szczegółowe instrukcje Konwertowanie elementu BoundField TemplateField i dodawanie kontrolek weryfikacji.


## <a name="summary"></a>Podsumowanie

Podczas definiowania schematu dla tabeli, programu Microsoft SQL Server umożliwia dołączenie kolumn obliczanych. Są to kolumn, których wartości są obliczane na podstawie wyrażenie, które zazwyczaj odwołuje się do wartości z innych kolumn w ten sam rekord. Od wartości dla kolumny obliczane są oparte na wyrażeniu, są tylko do odczytu i nie można przypisać wartości w `INSERT` lub `UPDATE` instrukcji. Wprowadza wyzwania, korzystając z kolumną obliczaną w główne zapytanie TableAdapter, który podejmie próbę automatycznego generowania odpowiadającego `INSERT`, `UPDATE`, i `DELETE` instrukcji.

W tym samouczku Omówiliśmy techniki obejściu wyzwania związanego z kolumnami obliczanymi. W szczególności użyliśmy procedur składowanych w naszym TableAdapter do pokonania kruchości związane z TableAdapters, użyj instrukcji SQL zapytań ad-hoc. Posiadanie TableAdapter Kreator tworzenia nowych procedur składowanych, jest ważne, że mamy główne zapytanie początkowo pominąć żadnej kolumny obliczanej, ponieważ ich występowanie uniemożliwia generowanych procedur przechowywanych modyfikacji danych. Po początkowym skonfigurowaniu TableAdapter jego `SelectCommand` procedury składowanej może być retooled do obejmuje żadnej kolumny obliczanej.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Hilton Geisenow i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-additional-datatable-columns-vb.md)
> [dalej](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
