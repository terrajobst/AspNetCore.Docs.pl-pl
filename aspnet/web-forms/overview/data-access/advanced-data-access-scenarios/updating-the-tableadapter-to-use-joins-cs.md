---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Aktualizowanie TableAdapter w celu użycia sprzężenia (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas pracy z bazą danych jest często stosowanym rozwiązaniem jest ich rozmieszczenie do kilku tabel danych żądania. Pobieranie danych z różnych tabel możemy użyć albo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: be74be8865b021be1f2e2d8181d2eb42cb74eb75
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Aktualizowanie TableAdapter w celu użycia sprzężenia (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) lub [pobierania plików PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Podczas pracy z bazą danych jest często stosowanym rozwiązaniem jest ich rozmieszczenie do kilku tabel danych żądania. Pobieranie danych z różnych tabel możemy użyć skorelowane podzapytania lub operacji tworzenia sprzężenia. W tym samouczku będziemy porównywanie skorelowane podzapytań i składni sprzężenia przed spojrzenie na tworzenie TableAdapter, zawierający sprzężenia w jej główne zapytanie.


## <a name="introduction"></a>Wprowadzenie

Z relacyjnych baz danych dane, które Dbamy o stosowaniu jest często rozłożyć na wiele tabel. Na przykład podczas wyświetlania informacji o produkcie prawdopodobnie chcemy każdego produktu s odpowiedniej kategorii i nazwy s dostawcy. `Products` Tabela ma `CategoryID` i `SupplierID` wartości, ale rzeczywiste nazwy kategorii i dostawcy znajdują się w `Categories` i `Suppliers` tabele, odpowiednio.

Aby pobrać informacje z innego, powiązanych tabel, możemy użyj *skorelowane podzapytania* lub `JOIN` *s*. Podzapytania skorelowane jest zagnieżdżoną `SELECT` kwerendę, która odwołuje się do kolumn w zapytaniu zewnętrzne. Na przykład w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) użyliśmy dwóch skorelowane podzapytań w samouczku `ProductsTableAdapter` s główne zapytanie do zwracania nazwy kategorii i dostawcy dla każdego produktu. A `JOIN` jest konstrukcja SQL, który scala powiązane wiersze z dwóch różnych tabel. My używamy `JOIN` w [badanie danych z formantem SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) samouczkiem, aby wyświetlić informacje o kategorii obok każdego produktu.

Przyczyna mieć możemy abstained korzystania z `JOIN` s TableAdapters jest ze względu na ograniczenia w Kreatorze s TableAdapter do automatycznego generowania odpowiadającego `INSERT`, `UPDATE`, i `DELETE` instrukcje. W szczególności jeśli obiekt TableAdapter s główne zapytanie zawiera którykolwiek `JOIN` s, TableAdapter nie może automatycznie twórz ad hoc instrukcji SQL lub procedur składowanych jego `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości.

W tym samouczku krótko porównamy i kontrastu skorelowane podzapytania i `JOIN` s przed rozpoczęciem pracy z TableAdapter, która obejmuje tworzenie `JOIN` s w jej główne zapytanie.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Porównywanie i kontrastem skorelowane podzapytania i`JOIN` s

Odwołania, który `ProductsTableAdapter` utworzony w pierwszym samouczku `Northwind` zestawu danych używa skorelowane podzapytania do przywrócenia nazwa każdego produktu s odpowiedniej kategorii i dostawcy. `ProductsTableAdapter` Poniżej przedstawiono główne zapytanie s.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Dwa skorelowane podzapytania - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` i `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -są `SELECT` zapytań zwracających pojedynczą wartość dla każdego produktu jako dodatkowe kolumny w zewnętrznym `SELECT` listy kolumn w instrukcji s.

Alternatywnie `JOIN` służy do zwracania nazwy każdego produktu s dostawcy i kategorii. Następujące zapytanie zwraca tych samych danych wyjściowych powyżej, ale używa `JOIN` s zamiast podzapytań:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

A `JOIN` scala rekordy z jednej tabeli z rekordów z innej tabeli na podstawie niektórych kryteriów. W powyższym zapytaniu, na przykład `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` powoduje, że program SQL Server do scalenia każdego rekordu produktu z kategorią rekordów, których `CategoryID` wartość odpowiada produktu s `CategoryID` wartość. Wynik scalonych pozwala pracować z odpowiednich pól kategorii dla każdego produktu (takie jak `CategoryName`).

> [!NOTE]
> `JOIN` s są często używane podczas wykonywania zapytań dotyczących danych relacyjnych baz danych. Jeśli jesteś nowym użytkownikiem `JOIN` składni lub konieczność lepiej poznać nieco jej użycie d najlepiej [samouczek SQL Join](http://www.w3schools.com/sql/sql_join.asp) w [szkoły W3](http://www.w3schools.com/). Warto odczytu są także [ `JOIN` podstawy](https://msdn.microsoft.com/library/ms191517.aspx) i [podstawy podzapytania](https://msdn.microsoft.com/library/ms189575.aspx) sekcje [SQL — książki Online](https://msdn.microsoft.com/library/ms130214.aspx).


Ponieważ `JOIN` s i podzapytania skorelowane mogą posłużyć do pobierania danych powiązanych z innych tabel, wielu deweloperów pozostało drapanie ich głowic i zastanawiać, które rozwiązanie do użycia. Wszystkie SQL gurus I Zapisz zawsze mówię do dostęp około samo, że t naprawdę istotne performance-wise sposób planów około identyczne wykonania programu SQL Server. Następnie rad, jest użycie metody, która Ciebie i Twojego zespołu są najbardziej Ci. Uzasadnia, biorąc pod uwagę, że po nadając tej porady tych ekspertów od razu express swoje preferencje `JOIN` s za pośrednictwem skorelowane podzapytania.

Podczas kompilowania Warstwa dostępu do danych, korzystając z wpisanych zestawów danych, narzędzia działał lepiej podczas korzystania z podzapytania. W szczególności TableAdapter Kreator s nie generowane automatycznie odpowiadającego `INSERT`, `UPDATE`, i `DELETE` instrukcje Jeśli główne zapytanie zawiera którykolwiek `JOIN` s, ale generowane automatycznie tych instrukcji, gdy skorelowane podzapytania są używane.

Aby zapoznać się z tego braku, tworzenie tymczasowego DataSet wpisane w `~/App_Code/DAL` folderu. W Kreatorze konfiguracji TableAdapter chce używać instrukcji SQL ad-hoc, a następnie wprowadź następujące `SELECT` kwerendy (zobacz rysunek 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Wprowadź główne zapytanie, która zawiera sprzężenia](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Rysunek 1**: Wprowadź kwerendę Main zawierający `JOIN` s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


Domyślnie automatycznie utworzy TableAdapter `INSERT`, `UPDATE`, i `DELETE` instrukcje na podstawie głównego zapytania. Jeśli klikniesz przycisk Zaawansowane widać, że ta funkcja jest włączona. Niezależnie od tego ustawienia TableAdapter nie będzie mógł tworzyć `INSERT`, `UPDATE`, i `DELETE` instrukcje ponieważ główne zapytanie zawiera `JOIN`.


![Wprowadź główne zapytanie, która zawiera sprzężenia](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Rysunek 2**: Wprowadź główne zapytanie, który zawiera `JOIN` s


Kliknij przycisk Zakończ, aby zakończyć pracę kreatora. W tym momencie z zestawu danych s projektanta będzie zawierać pojedynczy obiekt TableAdapter z obiektu DataTable z kolumn dla każdego pola zwracane w `SELECT` listy kolumny zapytania s. Obejmuje to `CategoryName` i `SupplierName`, jak pokazano na rysunku 3.


![Element DataTable zawiera kolumnę dla każdego pola zwrócił na liście kolumn](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Rysunek 3**: DataTable zawiera kolumnę dla każdego pola zwrócił na liście kolumn


Kiedy DataTable ma odpowiednie kolumn, TableAdapter nie ma wartości jego `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości. Można to potwierdzić, kliknij w TableAdapter w projektancie, a następnie przejdź do okna właściwości. Będzie informacja, że `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości są ustawione na (Brak).


[![Element InsertCommand UpdateCommand i właściwości elementu DeleteCommand są ustawione na (Brak)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Rysunek 4**: `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości są ustawione na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


W celu obejścia tego braku, można ręcznie udostępniamy instrukcje SQL i parametry `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości za pośrednictwem okna właściwości. Alternatywnie można uruchomić konfigurując TableAdapter s główne zapytanie do *nie* zawierać `JOIN` s. Dzięki temu będzie `INSERT`, `UPDATE`, i `DELETE` instrukcje, aby być automatycznie generowane w firmie Microsoft. Po zakończeniu pracy kreatora, firma Microsoft może następnie ręcznie zaktualizować TableAdapter s `SelectCommand` z okna właściwości, którą obejmuje `JOIN` składni.

Gdy ta metoda działa, jest bardzo łamliwa gdy za pomocą zapytań ad hoc SQL, ponieważ wtedy główne zapytanie TableAdapter s jest ponownie skonfigurować przy użyciu Kreatora automatycznego generowania `INSERT`, `UPDATE`, i `DELETE` instrukcje są tworzone ponownie. Oznacza to, że wszystkie modyfikacje później wprowadziliśmy zostałyby utracone jeśli firma Microsoft kliknął prawym przyciskiem myszy w metodzie TableAdapter, wybierz z menu kontekstowego Konfigurowanie i ponownie ukończono pracę kreatora.

Kruchości s TableAdapter automatycznie generowanej `INSERT`, `UPDATE`, i `DELETE` instrukcje jest jednak ograniczona do instrukcji SQL ad hoc. Jeśli Twoje TableAdapter używa procedur składowanych, można dostosować `SelectCommand`, `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` procedur składowanych i uruchom ponownie Kreatora konfiguracji TableAdapter bez obawy, będzie procedury składowane zmodyfikowane.

W następnych kilku kroków utworzymy TableAdapter, która początkowo używa główne zapytanie, które pomija wszelkie `JOIN` s, aby odpowiednie wstawiania, aktualizacji i usuwania przechowywane procedury zostanie wygenerowany automatycznie. Następnie modyfikacjom `SelectCommand` tak używa `JOIN` zwracającą dodatkowe kolumny z powiązanych tabel. Ponadto firma Microsoft będzie utworzyć odpowiadającą klasę warstwy logiki biznesowej i Wykaż, strony sieci web ASP.NET za pomocą TableAdapter.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Krok 1: Tworzenie za pomocą uproszczonego główne zapytanie TableAdapter

W tym samouczku dodamy TableAdapter i silnie typizowaną klasę DataTable `Employees` w tabeli `NorthwindWithSprocs` zestawu danych. `Employees` Tabela zawiera `ReportsTo` pola określającego `EmployeeID` menedżera pracownika s. Na przykład pracownik ma Dodsworth Anna `ReportTo` wartość 5, która jest `EmployeeID` z Nowak Steven. W rezultacie Anna raporty Steven, jego menedżera. Wraz z raportowania każdego pracownika s `ReportsTo` wartość, firma Microsoft może być również konieczne pobrać nazwy menedżera. Można to zrobić przy użyciu `JOIN`. Jednak przy użyciu `JOIN` podczas początkowo tworzenie TableAdapter wyklucza kreatora z automatycznego generowania odpowiednich wstawiania, aktualizowania i usuwania możliwości. W związku z tym firma Microsoft będzie Rozpocznij od utworzenia TableAdapter, którego główne zapytanie nie zawiera żadnych `JOIN` s. Następnie, w kroku 2 modyfikacjom główne zapytanie przechowywane procedury można pobrać nazwy menedżera s przy użyciu `JOIN`.

Uruchamianie przez otwarcie `NorthwindWithSprocs` zestaw danych z `~/App_Code/DAL` folderu. Kliknij prawym przyciskiem myszy w projektancie, wybierz opcję Dodaj z menu kontekstowego i wybierz element menu TableAdapter. Spowoduje to uruchomienie Kreatora konfiguracji TableAdapter. Jak przedstawia rysunek 5, za pomocą kreatora, Utwórz nowe procedury składowane i kliknij przycisk Dalej. Odświeżacz na tworzenie nowych przechowywanych procedur w Kreatorze s TableAdapter, zapoznaj się [Tworzenie nowej procedury składowanej s wpisane DataSet TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka.


[![Wybierz nowe procedury składowane Utwórz opcji](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Rysunek 5**: wybierz opcję tworzenia nowych przechowywane procedury opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Należy użyć następującego `SELECT` instrukcji dla Obiekt TableAdapter s główne zapytanie:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Ponieważ to zapytanie nie zawiera żadnych `JOIN` s, TableAdapter Kreator automatycznie utworzy procedur składowanych z odpowiednimi `INSERT`, `UPDATE`, i `DELETE` instrukcje, jak również procedury składowanej do wykonywania głównych Zapytanie.

Następny krok pozwala nazwę procedury przechowywane s TableAdapter. Użyj nazwy `Employees_Select`, `Employees_Insert`, `Employees_Update`, i `Employees_Delete`, jak pokazano na rysunku 6.


[![Nazwa procedury przechowywane s TableAdapter](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Rysunek 6**: Nazwa procedury składowane s TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


Ostatnim krokiem monituje nam w nazwie metody TableAdapter s. Użyj `Fill` i `GetEmployees` jako nazwy metody. Należy także pozostawić metody Create, aby bezpośrednio wysyłać aktualizacje zaznaczono element checkbox bazy danych (GenerateDBDirectMethods).


[![Nazwa metody wypełnienia s TableAdapter i GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Rysunek 7**: Nazwa s TableAdapter metody `Fill` i `GetEmployees` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Po zakończeniu działania kreatora Poświęć chwilę, aby zbadać procedur przechowywanych w bazie danych. Powinny pojawić się cztery nowe: `Employees_Select`, `Employees_Insert`, `Employees_Update`, i `Employees_Delete`. Następnie należy sprawdzić `EmployeesDataTable` i `EmployeesTableAdapter` właśnie utworzony. Element DataTable zawiera kolumnę dla każdego pola zwrócony przez główne zapytanie. Kliknij TableAdapter, a następnie przejdź do okna właściwości. Będzie informacja, że `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości są prawidłowo skonfigurowane do wywołania odpowiednich procedur składowanych.


[![TableAdapter obejmuje Insert, Update i usuwanie funkcji](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Rysunek 8**: TableAdapter obejmuje Insert, Update i usunąć możliwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


Z wstawiania, aktualizowania i usuwania procedur składowanych, automatycznie tworzony i `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości poprawnie skonfigurowane, możemy przystąpić do dostosowania `SelectCommand` s przechowywane procedury, aby przywrócić dodatkowe informacje dotyczące każdego menedżera pracownika s. W szczególności należy zaktualizować `Employees_Select` użyj procedury składowanej `JOIN` i zwracać Menedżera s `FirstName` i `LastName` wartości. Po zaktualizowaniu procedury składowanej, firma Microsoft wymaga zaktualizowania elementu DataTable, co zawierają one te dodatkowe kolumny. Firma Microsoft będzie spełnienia tych dwóch zadań w kroki 2 i 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Krok 2: Dostosowywanie procedury składowanej do uwzględnienia`JOIN`

Start, przechodząc do Eksploratora serwera, przechodzenie do folderu procedur składowanych s bazy danych Northwind i otwieranie `Employees_Select` procedury składowanej. Jeśli nie widzisz tej procedury składowanej, kliknij prawym przyciskiem folder procedur składowanych i kliknij przycisk Odśwież. Zaktualizuj procedury składowanej, tak aby były używane `LEFT JOIN` najpierw zwrócić Menedżera s i nazwisko:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Po zaktualizowaniu `SELECT` instrukcji, Zapisz zmiany, przechodząc do menu Plik i wybierając pozycję Zapisz `Employees_Select`. Alternatywnie można kliknąć ikonę Zapisz na pasku narzędzi, lub kliknij przycisk Ctrl + S. Po zapisaniu zmian, kliknij prawym przyciskiem myszy `Employees_Select` procedury składowanej w Eksploratorze serwera i wybierz polecenie Execute. Spowoduje to uruchom procedurę składowaną i wyświetlić wyniki w oknie danych wyjściowych (patrz rysunek 9).


[![W oknie dane wyjściowe są wyświetlane wyniki procedury przechowywane](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Rysunek 9**: wyników procedury przechowywane są wyświetlane w oknie danych wyjściowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Krok 3: Aktualizowanie kolumn s DataTable

W tym momencie `Employees_Select` przechowywane procedury zwraca `ManagerFirstName` i `ManagerLastName` wartości, ale `EmployeesDataTable` brakuje tych kolumn. Te brakujących kolumn mogą być dodawane do elementu DataTable w jeden z dwóch sposobów:

- **Ręcznie** — kliknij prawym przyciskiem myszy na DataTable w Projektancie obiektów DataSet i z menu Dodaj, wybierz kolumnę. Można następnie nazwy kolumny i odpowiednio ustawienia swoich właściwości.
- **Automatycznie** — Kreator konfiguracji TableAdapter będzie zaktualizować kolumn s DataTable, aby odzwierciedlić pola zwrócony przez `SelectCommand` procedury składowanej. Korzystając z instrukcji SQL ad-hoc, Kreator spowoduje również usunięcie `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości od `SelectCommand` zawiera teraz `JOIN`. Ale korzystając z procedur składowanych, te właściwości polecenia pozostaną nienaruszone.

Firma Microsoft ma przedstawione ręczne dodawanie kolumn do DataTable w poprzednim samouczki, w tym [wzorca/Detail, za pomocą listy punktowanej z rekordu głównego z DataList szczegóły](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) i [przesyłanie plików](../working-with-binary-files/uploading-files-cs.md), i zostaną wykonane następujące czynności Obejrzyj ten proces ponownie bardziej szczegółowo w naszym samouczku dalej. W tym samouczku jednak umożliwia s użyć metody automatycznego za pomocą Kreatora konfiguracji TableAdapter.

Uruchom, klikając prawym przyciskiem myszy `EmployeesTableAdapter` i wybierając pozycję Konfiguruj z menu kontekstowego. Spowoduje to wyświetlenie Kreatora konfiguracji TableAdapter, w którym wymieniono przechowywanych procedur wybranie, wstawianie, aktualizowanie i usuwanie, wraz z ich parametry (jeśli istnieje) i wartości zwracanych. Rysunek nr 10 przedstawia tego kreatora. W tym miejscu zobaczysz, że `Employees_Select` przechowywane procedury teraz zwraca `ManagerFirstName` i `ManagerLastName` pól.


[![Kreator pokazuje aktualizowanych kolumn na liście Employees_Select procedury składowanej](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Na rysunku nr 10**: Kreator z listą zaktualizować kolumny dla `Employees_Select` procedury składowanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Ukończ pracę kreatora, klikając przycisk Zakończ. Po powrocie do Projektanta obiektów DataSet `EmployeesDataTable` obejmuje dwie dodatkowe kolumny: `ManagerFirstName` i `ManagerLastName`.


[![EmployeesDataTable zawiera dwie nowe kolumny.](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Rysunek 11**: `EmployeesDataTable` zawiera dwie nowe kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Aby zilustrować, który zaktualizowanego `Employees_Select` procedury składowanej jest włączona i że wstawiania, aktualizowania i usuwania możliwości TableAdapter są nadal działa prawidłowo, niech s utworzyć stronę sieci web, który umożliwia użytkownikom wyświetlanie i usuwanie pracowników. Przed utworzymy strony sieci, należy najpierw utworzyć nową klasę w warstwy logiki biznesowej do pracy z pracowników z `NorthwindWithSprocs` zestawu danych. W kroku 4, utworzymy `EmployeesBLLWithSprocs` klasy. W kroku 5 użyjemy tej klasy ze strony programu ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Krok 4: Implementowanie warstwy logiki biznesowej

Utwórz nowy plik klasy w `~/App_Code/BLL` folder o nazwie `EmployeesBLLWithSprocs.cs`. Ta klasa naśladuje semantyki istniejącej `EmployeesBLL` klasy, tylko nowy jeden udostępnia metody mniej i używa `NorthwindWithSprocs` zestawu danych (zamiast `Northwind` zestawu danych). Dodaj następujący kod do `EmployeesBLLWithSprocs` klasy.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

`EmployeesBLLWithSprocs` Klasy s `Adapter` właściwość zwraca wystąpienie klasy `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. To jest używana przez klasę s `GetEmployees` i `DeleteEmployee` metody. `GetEmployees` Wywołania metody `EmployeesTableAdapter` odpowiadającego s `GetEmployees` metodę, która wywołuje `Employees_Select` procedury składowanej i wypełnia wyniki w `EmployeeDataTable`. `DeleteEmployee` Podobnie wywołuje metodę `EmployeesTableAdapter` s `Delete` metodę, która wywołuje `Employees_Delete` procedury składowanej.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Krok 5: Praca z danymi w warstwie prezentacji

Z `EmployeesBLLWithSprocs` klasy zakończone, możemy re gotowości do pracy z danymi pracowników za pośrednictwem strony platformy ASP.NET. Otwórz `JOINs.aspx` strony `AdvancedDAL` folder i przeciągnij element GridView z przybornika do projektanta, ustawienie jej `ID` właściwości `Employees`. Następnie z tagów inteligentnych s GridView powiązać siatki nowe kontrolki ObjectDataSource o nazwie `EmployeesDataSource`.

Element ObjectDataSource umożliwia konfigurowanie `EmployeesBLLWithSprocs` klasy, a na kartach wybierz i DELETE, upewnij się, że `GetEmployees` i `DeleteEmployee` metody są wybrany z listy rozwijanej. Kliknij przycisk Zakończ, aby zakończyć konfigurację ObjectDataSource s.


[![Skonfiguruj ObjectDataSource do użycia klasy EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Rysunek 12**: Konfigurowanie ObjectDataSource użyć `EmployeesBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![Mogą używać elementu ObjectDataSource GetEmployees i DeleteEmployee metody](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Rysunek 13**: mogą używać elementu ObjectDataSource `GetEmployees` i `DeleteEmployee` metod ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio spowoduje dodanie elementu BoundField do widoku GridView dla każdego z `EmployeesDataTable` s kolumn. Usuń wszystkie te BoundFields z wyjątkiem `Title`, `LastName`, `FirstName`, `ManagerFirstName`, i `ManagerLastName` i zmienić nazwę `HeaderText` właściwości cztery ostatnie BoundFields nazwisko, imię, imię Menedżera s i Menedżer s nazwisko, odpowiednio.

Aby zezwolić użytkownikom na usuwanie pracowników z tej strony, należy wykonać dwie czynności. Po pierwsze poinstruuj GridView zapewnienie możliwości usuwania zaznaczając opcję Włącz usuwanie z jego tagów inteligentnych. Po drugie, zmień ObjectDataSource s `OldValuesParameterFormatString` ustawić właściwości z wartością przez element ObjectDataSource kreatora (`original_{0}`) na wartość domyślną (`{0}`). Po wprowadzeniu tych zmian, z widoku GridView i ObjectDataSource s deklaratywne znaczników powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Przetestowanie strony, odwiedzając za pośrednictwem przeglądarki. Jak pokazano na rysunku 14, strona będzie zawierała listę poszczególnych pracowników i nazwy s własnego menedżera (przy założeniu, że mają one).


[![SPRZĘŻENIA w Employees_Select przechowywane procedury zwraca nazwę s Manager](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Rysunek 14**: `JOIN` w `Employees_Select` procedury przechowywanej zwraca Menedżera s Nazwa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


Kliknięcie przycisku Usuń rozpoczyna się usuwanie przepływu pracy, który culminates podczas wykonywania `Employees_Delete` procedury składowanej. Jednak próba `DELETE` instrukcji w procedurze składowanej zakończy się niepowodzeniem z powodu naruszenia ograniczenia klucza obcego (patrz rysunek 15). W szczególności każdy pracownik ma co najmniej jeden rekord `Orders` tabeli, co powoduje usunięcie niepowodzenie.


[![Usuwanie pracownik ma odpowiadające im wyniki zamówień w naruszenie ograniczenia klucza obcego](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Rysunek 15**: usuwanie pracownik ma odpowiadające im wyniki zamówień w naruszenie ograniczenia klucza obcego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Umożliwia pracownika usunięty użytkownik może:

- Aktualizacja kaskadowo usuwa, ograniczenie klucza obcego
- Ręcznie usuń rekordy z `Orders` tabeli dla pracowników chcesz usunąć, lub
- Aktualizacja `Employees_Delete` najpierw usunąć powiązane rekordy z procedury składowanej `Orders` tabeli przed usunięciem `Employees` rekordu. Omówiono tej techniki [za pomocą istniejących procedur składowanych s wpisane DataSet TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka.

I pozostaw to wykonywania dla czytnika.

## <a name="summary"></a>Podsumowanie

Podczas pracy z relacyjnych baz danych, często zapytania, aby pobierać dane z wielu powiązanych tabel. Skorelowane podzapytania i `JOIN` s Podaj dwie techniki uzyskiwania dostępu do danych z powiązanych tabel w zapytaniu. W poprzednich samouczki najczęściej wprowadziliśmy użycie podzapytań skorelowane ponieważ TableAdapter nie można automatycznie wygenerować `INSERT`, `UPDATE`, i `DELETE` instrukcje dotyczące zapytań `JOIN` s. Gdy te wartości można podać ręcznie, korzystając z instrukcji SQL ad hoc wszelkie dostosowania zostaną zastąpione po ukończeniu pracy Kreatora konfiguracji TableAdapter.

Na szczęście TableAdapters utworzone za pomocą procedur składowanych nie doświadczają kruchości sam, jak te utworzone za pomocą instrukcji SQL ad hoc. W związku z tym jest to możliwe tworzenie TableAdapter, którego główne zapytanie używa `JOIN` korzystając z procedur składowanych. W tym samouczku widzieliśmy tworzenie takich TableAdapter. Firma Microsoft uruchomiona przy użyciu `JOIN`-mniej `SELECT` zapytanie TableAdapter s główne zapytanie tak, że odpowiednie insert, update i delete przechowywane procedury utworzone automatycznie. Z TableAdapter s konfiguracji początkowej pełną, możemy rozszerzony `SelectCommand` użyj procedury składowanej `JOIN` i ponownie uruchomiono kreatora konfiguracji TableAdapter w celu zaktualizowania `EmployeesDataTable` s kolumn.

Ponowne uruchomienie Kreatora konfiguracji TableAdapter, automatycznie aktualizowane `EmployeesDataTable` kolumny, aby odzwierciedlić pola danych zwróconych przez `Employees_Select` procedury składowanej. Alternatywnie można dodaliśmy te kolumny ręcznie do obiektu DataTable. Przeanalizujemy ręczne dodawanie kolumn do DataTable w następnym samouczku.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Hilton Geisenow Suru Dominik i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [dalej](adding-additional-datatable-columns-cs.md)
