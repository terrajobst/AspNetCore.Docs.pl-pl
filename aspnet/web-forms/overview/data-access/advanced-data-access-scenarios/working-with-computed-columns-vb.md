---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Praca z kolumnami obliczanymi (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas tworzenia tabeli bazy danych programu Microsoft SQL Server można zdefiniować kolumną obliczaną, którego wartość jest obliczana na podstawie wyrażenia to zwykle referen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 04b39902aae05d815eb11ec7b7163988d017f78c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="working-with-computed-columns-vb"></a>Praca z kolumnami obliczanymi (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) lub [pobierania plików PDF](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Podczas tworzenia tabeli bazy danych programu Microsoft SQL Server można zdefiniować kolumną obliczaną, którego wartość jest obliczana na podstawie wyrażenie, które zazwyczaj odwołuje się do innych wartości tego samego rekordu bazy danych. Wartości te są tylko do odczytu w bazie danych, które wymaga uwagi, podczas pracy z TableAdapters. W tym samouczku będziemy Dowiedz się, jak spełnia wyzwanie spowodowane kolumn obliczanych.


## <a name="introduction"></a>Wprowadzenie

Microsoft SQL Server umożliwia  *[kolumny obliczane](https://msdn.microsoft.com/library/ms191250.aspx)*, które są kolumny, których wartości są obliczane na podstawie wyrażenie, które zazwyczaj odwołuje się do wartości z innych kolumn w tej samej tabeli. Na przykład czasu, śledzenie modelu danych może być tabela o nazwie `ServiceLog` z kolumnami tym `ServicePerformed`, `EmployeeID`, `Rate`, i `Duration`, między innymi. Podczas należności na usługę elementu (szybkość pomnożona przez czas trwania) może być obliczenie za pośrednictwem strony sieci web lub inny interfejs programistyczny, może być przydatne, aby zawierała kolumnę w `ServiceLog` tabeli o nazwie `AmountDue` który zgłosił to informacje. Mógł zostać utworzony w tej kolumnie jako normalne kolumny, ale musi on zostać zaktualizowany w dowolnym momencie `Rate` lub `Duration` zmienione wartości w kolumnie. Lepszym rozwiązaniem byłoby dokonanie `AmountDue` przy użyciu wyrażenia kolumny obliczanej kolumny `Rate * Duration`. To spowodowałoby wystąpienie programu SQL Server, można automatycznie obliczyć `AmountDue` wartość kolumny zawsze, gdy został do niego odwołania w zapytaniu.

Ponieważ wartość kolumny obliczanej s jest określany przez wyrażenie, takich kolumn są tylko do odczytu i dlatego nie można przypisać wartość je w `INSERT` lub `UPDATE` instrukcje. Jednak gdy kolumny obliczane są częścią główne zapytanie dla Obiekt TableAdapter, która używa instrukcji SQL ad-hoc, zostaną one automatycznie uwzględnione w generowanych automatycznie `INSERT` i `UPDATE` instrukcje. W rezultacie s TableAdapter `INSERT` i `UPDATE` zapytania i `InsertCommand` i `UpdateCommand` można zaktualizować właściwości, aby usunąć odwołania do kolumn obliczanych.

Jednym z wyzwań przy użyciu kolumny za pomocą TableAdapter, która używa instrukcji SQL ad hoc obliczane jest to, że TableAdapter s `INSERT` i `UPDATE` zapytań automatycznie są generowane w dowolnym momencie ukończeniu pracy Kreatora konfiguracji TableAdapter. W związku z tym kolumn obliczanych ręcznie usunięty z `INSERT` i `UPDATE` zapytań pojawi się ponownie, jeśli jest ponownie uruchom kreatora. Mimo że ADAM TableAdapters korzystających z procedur składowanych t boryka się z tym kruchości, mają własne Osobliwości, które będzie można rozwiązać w kroku 3.

W tym samouczku zostanie dodany do kolumny obliczanej `Suppliers` tabeli w bazie danych Northwind, a następnie utwórz odpowiedni obiekt TableAdapter do pracy z tej tabeli i jej kolumny obliczanej. Mamy naszych TableAdapter używane procedury składowane zamiast instrukcji SQL ad-hoc, aby nasze dostosowania nie są t utracone podczas służy Kreator konfiguracji TableAdapter.

Rozpoczynanie pracy dzięki s!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Krok 1: Dodawanie kolumny obliczanej do`Suppliers`tabeli

Baza danych Northwind nie ma żadnych kolumn obliczanych, więc musimy dodać jeden nad. Dla tego samouczka let s dodać kolumny obliczanej do `Suppliers` tabeli o nazwie `FullContactName` zwracającą nazwę kontaktu s, tytułu prawnego pracują w firmie w następującym formacie: `ContactName` (`ContactTitle`, `CompanyName`). To obliczona kolumn mogą być używane w raportach, podczas wyświetlania informacji o dostawcy.

Uruchamianie przez otwarcie `Suppliers` definicja tabeli, klikając prawym przyciskiem myszy `Suppliers` tabeli w Eksploratorze serwera i wybierając z menu kontekstowego Otwórz definicję tabeli. Spowoduje to wyświetlenie kolumn tabeli i ich właściwości, takie jak nazwa ich typu danych, czy umożliwiają one `NULL` s i tak dalej. Aby dodać kolumnę obliczaną, uruchom wpisując nazwę kolumny w definicji tabeli. Następnie wprowadź jej wyrażenia w polu tekstowym (Formuła) w sekcji specyfikację obliczanej kolumny w oknie właściwości kolumny (zobacz rysunek 1). Nazwa kolumny obliczanej `FullContactName` i należy użyć następującego wyrażenia:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Należy pamiętać, że może zostać dołączona ciągów w programie SQL przy użyciu `+` operatora. `CASE` Instrukcji można użyć takich jak warunkowego tradycyjnego języka programowania. W wyrażeniu powyżej `CASE` instrukcja może zostać odczytany jako: Jeśli `ContactTitle` nie jest `NULL` następnie output `ContactTitle` połączony z przecinkami, w przeciwnym razie wartość Emituj nothing. Aby uzyskać więcej informacji na temat przydatność `CASE` instrukcji, zobacz [Power SQL `CASE` instrukcje](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Zamiast `CASE` instrukcji w tym miejscu, można też użyliśmy `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Zwraca *checkExpression* przypadku jest inne niż NULL, w przeciwnym razie zwraca *replacementValue*. Podczas albo `ISNULL` lub `CASE` będzie działać w tym wystąpieniu są bardziej skomplikowanych scenariusze której elastyczność `CASE` instrukcji nie można dopasować przez `ISNULL`.


Po dodaniu tę kolumnę obliczaną na ekranie powinna wyglądać ekranu zrzut na rysunku 1.


[![Dodaj kolumnę obliczaną o nazwie FullContactName do tabeli dostawcy](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Rysunek 1**: dodać obliczanej kolumny o nazwie `FullContactName` do `Suppliers` tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image3.png))


Po nazewnictwa kolumna obliczana i wprowadzając jej wyrażenia, zapisać zmiany w tabeli, klikając ikonę Zapisz na pasku narzędzi, przez naciśnięcie klawiszy Ctrl + S lub przechodząc do menu Plik i wybierając pozycję Zapisz `Suppliers`.

Zapisywanie tabeli należy odświeżyć Eksploratora serwera, po prostu dodane kolumny w tym `Suppliers` listy kolumn tabeli s. Ponadto wyrażenie wprowadzona w polu tekstowym (formuły) spowoduje automatyczne dopasowanie równoważne wyrażenie, które usuwa niepotrzebne odstępu, otaczający nazwy kolumn w nawiasy (`[]`) i zawiera nawiasy, aby wyświetlić więcej jawnie kolejność operacji:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Aby uzyskać więcej informacji o kolumnach obliczanych w programie Microsoft SQL Server, zapoznaj się [dokumentacji technicznej](https://msdn.microsoft.com/library/ms191250.aspx). Sprawdź również [porady: Określ kolumny obliczanej](https://msdn.microsoft.com/library/ms188300.aspx) przewodnik krok po kroku tworzenia kolumn obliczanych.

> [!NOTE]
> Domyślnie kolumnach obliczanych fizycznie nie są przechowywane w tabeli, ale zamiast tego są obliczenia zawsze, gdy odwołuje się do zapytania. Jednak, zaznaczając pole wyboru jest trwały, można nakazać SQL Server, aby fizycznie przechowywania kolumny obliczanej w tabeli. Dzięki temu indeks ma zostać utworzony w kolumnie obliczanej, co może poprawić wydajność zapytań, które należy użyć wartości kolumny obliczanej w ich `WHERE` klauzul. Zobacz [tworzenie indeksów dla kolumny obliczanej](https://msdn.microsoft.com/library/ms189292.aspx) Aby uzyskać więcej informacji.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Krok 2: Przeglądanie kolumny obliczanej wartości s

Przed Rozpoczniemy pracy na warstwie dostępu do danych umożliwiają s zająć kilka minut, aby wyświetlić `FullContactName` wartości. Z poziomu Eksploratora serwera, kliknij prawym przyciskiem myszy `Suppliers` Nazwa tabeli i z menu kontekstowego wybierz pozycję Nowa kwerenda. Pojawi się okno kwerendy z monitem o nas o wybierz tabele do uwzględnienia w zapytaniu. Dodaj `Suppliers` tabeli, a następnie kliknij przycisk Zamknij. Następnie sprawdź `CompanyName`, `ContactName`, `ContactTitle`, i `FullContactName` kolumny z tabeli dostawcy. Na koniec kliknij ikony czerwonego wykrzyknika w pasku narzędzi, aby wykonać zapytanie i wyświetlić wyniki.

Jak pokazano na rysunku 2, wyniki będą zawierać `FullContactName`, które listy `CompanyName`, `ContactName`, i `ContactTitle` kolumn w formacie `ContactName` (`ContactTitle`, `CompanyName`).


[![FullContactName używa przedstawiciel formatu (StanowiskoPrzedstawiciela, NazwaFirmy)](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Rysunek 2**: `FullContactName` używa formatu `ContactName` (`ContactTitle`, `CompanyName`) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Krok 3: Dodawanie`SuppliersTableAdapter`do Warstwa dostępu do danych

Aby pracować z informacjami o dostawcy w naszej aplikacji należy najpierw utworzyć w naszym DAL TableAdapter i elementu DataTable. Najlepszym rozwiązaniem to czy osiągnąć w starszych samouczki proste kroki. Jednak w pracy z kolumnami obliczanymi wprowadzono kilka zagnieceń, które wymagają dyskusji.

Jeśli używasz TableAdapter, która używa instrukcji SQL ad-hoc, po prostu można uwzględnić kolumny obliczanej w TableAdapter s główne zapytanie za pomocą Kreatora konfiguracji TableAdapter. To, jednak generowane automatycznie `INSERT` i `UPDATE` instrukcji, które obejmują kolumny obliczanej. Jeśli użytkownik spróbuje wykonać jedną z następujących metod `SqlException` komunikat kolumny o *ColumnName* nie można zmodyfikować, ponieważ jest to kolumna obliczana lub znajduje się wynik operatora UNION, zostanie zgłoszony. Gdy `INSERT` i `UPDATE` instrukcji można ręcznie dostosować za pomocą TableAdapter s `InsertCommand` i `UpdateCommand` właściwości, te modyfikacje zostaną utracone zawsze, gdy jest ponownie uruchom Kreatora konfiguracji TableAdapter.

Z powodu kruchości TableAdapters używanego przez instrukcje SQL ad-hoc zaleca się podczas pracy z kolumnami obliczanymi możemy użyć procedur składowanych. Jeśli korzystasz z istniejącymi procedurami składowanymi, po prostu skonfiguruj TableAdapter zgodnie z opisem w [za pomocą istniejących procedur składowanych s wpisane DataSet TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) samouczka. Jeśli masz TableAdapter kreator Utwórz procedury składowane można jednak jest ważne pominąć początkowo żadnych kolumn obliczanych w głównym zapytaniu. Jeśli główne zapytanie zawiera kolumnę obliczaną, Kreator konfiguracji TableAdapter informuje, po zakończeniu, nie może utworzyć odpowiednich procedur składowanych. Krótko mówiąc, należy wstępnie skonfigurować przy użyciu obliczanej kolumny bez główne zapytanie TableAdapter, a następnie ręcznie zaktualizuj odpowiednich procedur składowanych i TableAdapter s `SelectCommand` aby zawierała kolumnę obliczaną. Ta metoda jest podobna do używaną w [aktualizowanie TableAdapter w celu użycia](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* samouczka.

W tym samouczku umożliwiają s, Dodaj nowy obiekt TableAdapter i go automatycznie utworzyć procedur składowanych w firmie Microsoft. W związku z tym, musimy początkowo Pomiń `FullContactName` kolumny obliczanej w głównym zapytaniu.

Uruchamianie przez otwarcie `NorthwindWithSprocs` zestaw danych z `~/App_Code/DAL` folderu. Kliknij prawym przyciskiem myszy w projektancie, a następnie z menu kontekstowego wybierz dodać nowy obiekt TableAdapter. Spowoduje to uruchomienie Kreatora konfiguracji TableAdapter. Określanie wykonać zapytania o dane z bazy danych (`NORTHWNDConnectionString` z `Web.config`) i kliknij przycisk Dalej. Ponieważ firma Microsoft nie utworzono jeszcze żadnych procedur składowanych do badania lub modyfikowania `Suppliers` tabeli, wybierz opcję Utwórz nowe procedury składowane opcję, aby kreator Utwórz je firmie Microsoft i kliknij przycisk Dalej.


[![Wybierz nowe procedury składowane Utwórz opcji](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Rysunek 3**: Wybierz nowe procedury składowane Utwórz opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image9.png))


Kolejnym kroku nam wyświetli monit o główne zapytanie. Wprowadź następujące zapytanie zwraca `SupplierID`, `CompanyName`, `ContactName`, i `ContactTitle` kolumn dla każdego dostawcy. Należy pamiętać, że to zapytanie celowo pomija kolumny obliczanej (`FullContactName`); modyfikacjom odpowiednie procedury składowanej, aby zawierało tej kolumny w kroku 4.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Po wprowadzeniu główne zapytanie, a następnie klikając przycisk Dalej, Kreator pozwala nazwa cztery procedur składowanych, który zostanie wygenerowany. Nazwa procedury składowanej te `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, i `Suppliers_Delete`, tak jak pokazano na rysunku 4.


[![Dostosowywanie nazw automatycznego generowania procedur składowanych](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Rysunek 4**: Dostosowywanie nazw wygenerowanej przechowywanych procedur ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image12.png))


Następny krok kreatora pozwala nazwy metody TableAdapter s i określ wzorce używane do pobrania i aktualizacji danych. Pozostaw zaznaczone wszystkie trzy pola wyboru, ale zmiana nazwy `GetData` metodę `GetSuppliers`. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Zmień GetSuppliers GetData — metoda](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Rysunek 5**: Zmień nazwę `GetData` metodę `GetSuppliers` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image15.png))


Po kliknięciu pozycji Zakończ program Kreator Utwórz cztery procedury składowane i dodać TableAdapter i odpowiedniego elementu DataTable wpisane DataSet.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Krok 4: W tym kolumny obliczanej w głównym zapytaniu s TableAdapter

Teraz należy zaktualizować TableAdapter i DataTable utworzony w kroku 3, aby uwzględnić `FullContactName` kolumny obliczanej. Obejmuje to wykonać dwie czynności:

1. Aktualizowanie `Suppliers_Select` przechowywane procedury, aby zwrócić `FullContactName` kolumna obliczana i
2. Aktualizowanie elementu DataTable do dołączenia do odpowiadającego `FullContactName` kolumny.

Uruchom, przechodząc do Eksploratora serwera i przechodzenie do folderu procedur składowanych. Otwórz `Suppliers_Select` przechowywane procedury i aktualizacji `SELECT` zapytanie zawierało `FullContactName` kolumny obliczanej:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Zapisać zmiany do procedury składowanej, klikając ikonę Zapisz na pasku narzędzi, przez naciśnięcie klawiszy Ctrl + S lub wybierz polecenie Zapisz `Suppliers_Select` opcji z menu Plik.

Następnie wróć do Projektanta obiektów DataSet, kliknij prawym przyciskiem myszy `SuppliersTableAdapter`i wybierz opcję Konfiguruj z menu kontekstowego. Należy pamiętać, że `Suppliers_Select` zawiera obecnie kolumnę `FullContactName` kolumny w kolekcji jego kolumn danych.


[![Uruchom Kreatora konfiguracji TableAdapter s można zaktualizować kolumn s DataTable](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Rysunek 6**: uruchom s TableAdapter Kreator konfiguracji można zaktualizować elementu DataTable s kolumn ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image18.png))


Kliknij przycisk Zakończ, aby zakończyć pracę kreatora. Automatycznie doda odpowiadającej mu kolumny do `SuppliersDataTable`. TableAdapter Kreator jest inteligentne wykryć, że `FullContactName` kolumna jest kolumną obliczaną, a więc tylko do odczytu. W związku z tym, ustawia kolumny s `ReadOnly` właściwości `true`. Aby to sprawdzić, należy zaznaczyć kolumnę z `SuppliersDataTable` , a następnie przejdź do okna właściwości (patrz rysunek 7). Należy pamiętać, że `FullContactName` kolumny s `DataType` i `MaxLength` właściwości również są ustawione w związku z tym.


[![Kolumna FullContactName jest oznaczony jako tylko do odczytu](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Rysunek 7**: `FullContactName` kolumna jest oznaczona jako tylko do odczytu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Krok 5: Dodawanie`GetSupplierBySupplierID`metody TableAdapter

W tym samouczku utworzymy strony ASP.NET, która wyświetla dostawców w siatce można aktualizować. W przeszłości samouczki zostały zaktualizowane pojedynczy rekord z warstwy logiki biznesowej przez pobranie, czy określony rekord z warstwy DAL jako jednoznacznie DataTable, aktualizowanie jego właściwości, a następnie wysyła zaktualizowane DataTable z powrotem do warstwy DAL propagowanie zmian Baza danych. Aby wykonać ten krok pierwszy - pobierania rekordu aktualizowana z warstwy DAL - musimy najpierw dodać `GetSupplierBySupplierID(supplierID)` metody z warstwą dal.

Kliknij prawym przyciskiem myszy `SuppliersTableAdapter` w projekcie DataSet i wybierz opcję Dodaj zapytanie z menu kontekstowego. Jak robiliśmy w kroku 3 pozwolić, aby wygenerować nową procedurę składowaną firmie Microsoft, wybierając opcję Utwórz nowe procedury składowanej Kreator (odwołują się do rysunku 3 dla tego kroku kreatora zrzut ekranu). Ponieważ ta metoda zwraca rekord z wieloma kolumnami, wskazuje chęć Użyj kwerendę SQL SELECT, która zwraca wiersze, a następnie kliknij przycisk Dalej.


[![Wybierz SELECT, która zwraca wiersze opcji](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Rysunek 8**: Wybierz SELECT, która zwraca wiersze opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image24.png))


Kolejnym kroku monituje o nas dla zapytania użyć tej metody. Wprowadź następujące polecenie, które zwraca tych samych polach danych jako głównej kwerendy, ale dla określonego dostawcy.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

Następnym ekranie zapyta nam nazwę procedury składowanej, która zostanie wygenerowana automatycznie. Nazwa tej procedury składowanej `Suppliers_SelectBySupplierID` i kliknij przycisk Dalej.


[![Nazwa procedury składowanej Suppliers_SelectBySupplierID](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Rysunek 9**: Nazwa procedury składowanej `Suppliers_SelectBySupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image27.png))


Ponadto Kreator monity nam danych dostęp wzorców i nazwy metod służących do TableAdapter. Pozostaw oba pola wyboru zaznaczone, ale zmiana nazwy `FillBy` i `GetDataBy` metody `FillBySupplierID` i `GetSupplierBySupplierID`odpowiednio.


[![Nazwa FillBySupplierID metod TableAdapter i GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Na rysunku nr 10**: Nazwa metody TableAdapter `FillBySupplierID` i `GetSupplierBySupplierID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image30.png))


Kliknij przycisk Zakończ, aby zakończyć pracę kreatora.

## <a name="step-6-creating-the-business-logic-layer"></a>Krok 6: Tworzenie warstwy logiki biznesowej

Przed utworzymy strony platformy ASP.NET, który używa kolumny obliczanej utworzone w kroku 1, najpierw należy dodać odpowiednich metod w logiki warstwy Biznesowej. Strony ASP.NET, który zostanie utworzony w kroku 7, pozwoli użytkownikom na wyświetlanie i edytowanie dostawcy. W związku z tym potrzebujemy naszych logiki warstwy Biznesowej, aby zapewnić co najmniej metody dla wszystkich dostawców i innej aktualizacji danego dostawcy.

Utwórz nowy plik klasy o nazwie `SuppliersBLLWithSprocs` w `~/App_Code/BLL` folderu i Dodaj następujący kod:


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Inne klasy logiki warstwy Biznesowej, takich jak `SuppliersBLLWithSprocs` ma `Protected` `Adapter` właściwość, która zwraca wystąpienie klasy `SuppliersTableAdapter` klasy wraz z dwóch `Public` metody: `GetSuppliers` i `UpdateSupplier`. `GetSuppliers` Metoda wywołuje i zwraca `SuppliersDataTable` zwrócony przez odpowiednie `GetSupplier` metody w warstwie dostępu do danych. `UpdateSupplier` Metoda pobiera informacji na temat określonego dostawcę aktualizowana rozmów na DAL s `GetSupplierBySupplierID(supplierID)` metody. Następnie aktualizuje `CategoryName`, `ContactName`, i `ContactTitle` właściwości i zatwierdza zmiany w bazie danych, wywołując s Warstwa dostępu do danych `Update` metody, przekazując zmodyfikowanych `SuppliersRow` obiektu.

> [!NOTE]
> Z wyjątkiem `SupplierID` i `CompanyName`, Zezwalaj na wszystkie kolumny w tabeli dostawcy `NULL` wartości. W związku z tym jeśli przekazany do `contactName` lub `contactTitle` parametry są `Nothing` należy ustawić odpowiednie `ContactName` i `ContactTitle` właściwości `NULL` przy użyciu wartości bazy danych `SetContactNameNull` i `SetContactTitleNull`metod, odpowiednio.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Krok 7: Praca z kolumny obliczanej z warstwy prezentacji

Z kolumną obliczaną, dodane do `Suppliers` tabeli i DAL i logiki warstwy Biznesowej odpowiednio aktualizowany, możemy rozpocząć tworzenie strony platformy ASP.NET, który działa z `FullContactName` kolumny obliczanej. Uruchamianie przez otwarcie `ComputedColumns.aspx` strony `AdvancedDAL` folder i przeciągnij element GridView z przybornika do projektanta. Ustaw GridView s `ID` właściwości `Suppliers` i z jego tagów inteligentnych powiązać go z nowego elementu ObjectDataSource o nazwie `SuppliersDataSource`. Element ObjectDataSource umożliwia konfigurowanie `SuppliersBLLWithSprocs` klasy dodaliśmy zwrotnie w kroku 6 i kliknij przycisk Dalej.


[![Skonfiguruj ObjectDataSource do użycia klasy SuppliersBLLWithSprocs](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Rysunek 11**: Konfigurowanie ObjectDataSource użyć `SuppliersBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image33.png))


Istnieją tylko dwa metody zdefiniowane w `SuppliersBLLWithSprocs` klasy: `GetSuppliers` i `UpdateSupplier`. Upewnij się, że te dwie metody są określone w polu Wybierz odpowiednio zaktualizować karty i kliknij przycisk Zakończ, aby zakończyć konfigurację elementu ObjectDataSource.

Po zakończeniu działania Kreatora konfiguracji źródła danych Visual Studio spowoduje dodanie elementu BoundField dla każdego pola danych zwracane. Usuń `SupplierID` elementu BoundField i zmień `HeaderText` właściwości `CompanyName`, `ContactName`, `ContactTitle`, i `FullContactName` BoundFields do firmy, skontaktuj się z nazwy, tytuł i Pełna nazwa kontaktu, odpowiednio. Z tagów inteligentnych zaznacz pole wyboru Włącz edytowanie, aby włączyć GridView s wbudowanych możliwości edycji.

Oprócz dodania BoundFields do widoku GridView, zakończeniu działania Kreatora źródła danych powoduje także, że Visual Studio, aby ustawić ObjectDataSource s `OldValuesParameterFormatString` właściwości z oryginalną\_{0}. Przywróć tę zmianę jego wartość domyślna, {0}.

Po wprowadzeniu tych zmian do widoku GridView i ObjectDataSource, ich deklaratywne znaczników powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Następnie odwiedź stronę tej strony za pośrednictwem przeglądarki. Jak pokazano na rysunku 12, każdy dostawca znajduje się w siatce, która obejmuje `FullContactName` kolumny, której wartość jest po prostu łączenia trzy kolumny w formacie `ContactName` (`ContactTitle`, `CompanyName`).


[![Każdy dostawca jest wymieniona w siatce](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Rysunek 12**: każdy dostawca jest wymieniona w siatce ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image36.png))


Kliknięcie przycisku Edytuj dla określonego dostawcę powoduje odświeżenie strony i ma renderowania tego wiersza, w jego edycji interfejsu (patrz rysunek 13). Pierwsze trzy kolumny renderowania w swojej domyślnej edycji interfejsu — pole tekstowe kontroli, których `Text` właściwości ustawiono wartość w polu danych. `FullContactName` Kolumny, jednak nadal jako tekst. Gdy BoundFields zostały dodane do widoku GridView po zakończeniu działania Kreator konfiguracji źródła danych `FullContactName` s elementu BoundField `ReadOnly` ustawiono właściwość `True` ponieważ odpowiadającego `FullContactName` kolumny w `SuppliersDataTable` ma jego `ReadOnly` ustawioną właściwość `True`. Zgodnie z opisem w kroku 4 `FullContactName` s `ReadOnly` ustawiono właściwość `True` ponieważ TableAdapter wykrył, że kolumna jest kolumną obliczaną.


[![Nie można edytować jest kolumny FullContactName](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Rysunek 13**: `FullContactName` kolumna jest nieedytowalny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](working-with-computed-columns-vb/_static/image39.png))


Przejdź dalej i zaktualizuj wartość co najmniej jedna z kolumn można edytować i kliknij przycisk Aktualizuj. Uwaga jak `FullContactName` s wartość zostanie automatycznie zaktualizowana w celu odzwierciedlenia zmian.

> [!NOTE]
> Widoku GridView aktualnie używa BoundFields dla edytowalnego pola, co w domyślnym edycji interfejsu. Ponieważ `CompanyName` pole jest wymagane, powinny być konwertowane na pole TemplateField, która obejmuje RequiredFieldValidator. I pozostaw to wykonywania zainteresowanych czytnika. Zapoznaj się [dodawanie formantów weryfikacji do edycji i wstawianie interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) samouczka instrukcje krok po kroku na Konwertowanie elementu BoundField na pole TemplateField i dodawanie formantów sprawdzania poprawności.


## <a name="summary"></a>Podsumowanie

Podczas definiowania schematu dla tabeli, programu Microsoft SQL Server umożliwia włączenie kolumn obliczanych. To są kolumny, których wartości są obliczane na podstawie wyrażenie, które zazwyczaj odwołuje się do wartości z innych kolumn w ten sam rekord. Począwszy od wartości dla kolumny obliczane są oparte na wyrażeniu, ich są tylko do odczytu i nie można przypisać wartości w `INSERT` lub `UPDATE` instrukcji. Wprowadza wyzwania, korzystając z kolumną obliczaną w główne zapytanie TableAdapter, która podejmuje próbę automatycznego generowania odpowiadającego `INSERT`, `UPDATE`, i `DELETE` instrukcje.

W tym samouczku omówiono techniki obejścia wyzwanie spowodowane kolumn obliczanych. W szczególności użyliśmy procedur składowanych w naszym TableAdapter usunie kruchości w TableAdapters używanego przez instrukcje SQL ad hoc. O TableAdapter Kreator tworzenia nowych procedur składowanych jest ważne, czy mamy główne zapytanie początkowo pominąć wszystkie kolumny obliczane, ponieważ ich obecność uniemożliwia generowaną procedury przechowywane modyfikacji danych. Po początkowym skonfigurowaniu TableAdapter jego `SelectCommand` procedurę składowaną można retooled uwzględnienie żadnych kolumn obliczanych.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Hilton Geisenow i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](adding-additional-datatable-columns-vb.md)
> [dalej](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
