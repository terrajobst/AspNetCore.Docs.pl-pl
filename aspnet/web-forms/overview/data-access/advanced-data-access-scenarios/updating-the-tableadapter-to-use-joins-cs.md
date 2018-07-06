---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Aktualizowanie elementu TableAdapter w celu używania sprzężenia (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Podczas pracy z bazą danych jest często dane żądania, która jest rozłożona się między wieloma tabelami. Aby pobrać dane z dwóch różnych tabel możemy użyć albo...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: c59ab13eb5e5e548055c6c8c9343a8d5c4cabe7d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841124"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Aktualizowanie elementu TableAdapter w celu używania sprzężenia (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) lub [Pobierz plik PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Podczas pracy z bazą danych jest często dane żądania, która jest rozłożona się między wieloma tabelami. Aby pobrać dane z dwóch różnych tabel możemy użyć skorelowane podzapytanie lub operacji tworzenia sprzężenia. W tym samouczku porównamy skorelowany podzapytań i składni klauzuli JOIN przed obejrzeniem sposób tworzenia, zawierający sprzężenia w jego główne zapytanie TableAdapter.


## <a name="introduction"></a>Wprowadzenie

Przy użyciu relacyjnych baz danych dane, które jesteśmy zainteresowani Praca z często rozkłada się na wiele tabel. Na przykład podczas wyświetlania informacji o produkcie prawdopodobnie chcemy wyświetlić listę każdego produktu s odpowiedniej kategorii i nazw s dostawców. `Products` Tabela ma `CategoryID` i `SupplierID` wartości, ale rzeczywiste nazwy kategorii i dostawcy znajdują się w `Categories` i `Suppliers` tabel, odpowiednio.

Aby pobrać informacje z inną, powiązanej tabeli, możemy użyć *skorelowane podzapytań* lub `JOIN` *s*. Skorelowane podzapytanie jest zagnieżdżoną `SELECT` zapytania, który odwołuje się do kolumn w zapytaniu zewnętrznego. Na przykład w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) samouczka użyliśmy dwóch podzapytań skorelowane w `ProductsTableAdapter` s główne zapytanie, aby zwrócić nazwy kategorii i dostawcy dla każdego produktu. Element `JOIN` jest konstrukcją SQL, która scala powiązane wiersze z dwóch różnych tabel. Użyliśmy `JOIN` w [zapytania danych przy użyciu kontrolki SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) samouczka, aby wyświetlić informacje o kategorii obok każdego produktu.

Przyczyna, firma Microsoft ma abstained z przy użyciu `JOIN` s przy użyciu elementów TableAdapter jest ze względu na ograniczenia w Kreatorze s TableAdapter, aby automatycznie wygenerować odpowiadającą `INSERT`, `UPDATE`, i `DELETE` instrukcji. W szczególności jeśli TableAdapter s główne zapytanie zawiera którykolwiek `JOIN` s, TableAdapter nie automatyczne tworzenie zapytań ad-hoc instrukcji SQL lub procedur składowanych do jego `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości.

W tym samouczku krótko porównamy i kontrast skorelowane podzapytań i `JOIN` s przed rozpoczęciem pracy, jak utworzyć obiekt TableAdapter, która obejmuje `JOIN` s w jego główne zapytanie.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Porównywanie i kontrastujących skorelowane podzapytań i`JOIN` s

Pamiętamy `ProductsTableAdapter` utworzony w pierwszym samouczku `Northwind` zestawu danych używa skorelowany podzapytań przywrócić nazwy każdego produktu s odpowiedniej kategorii i dostawcy. `ProductsTableAdapter` Poniżej przedstawiono główne zapytanie s.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Dwa skorelowane podzapytań - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` i `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` — są `SELECT` zapytań zwracających pojedynczą wartość dla każdego produktu jako dodatkową kolumnę w zewnętrzny blok `SELECT` instrukcja s lista kolumn.

Alternatywnie `JOIN` może służyć do zwrotu nazwy każdego produktu s dostawcy i kategorii. Następujące zapytanie zwraca ten sam wynik co powyżej, ale używa `JOIN` s zamiast podzapytań:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

A `JOIN` scala rekordy z jednej tabeli rekordy z innej tabeli na podstawie pewnych kryteriów. W powyższym zapytaniu, na przykład `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` powoduje, że program SQL Server do scalenia każdego rekordu produktu z kategorii rejestrowania, którego `CategoryID` wartość odpowiada produkt s `CategoryID` wartość. Scalonego wyniku pozwala pracować z odpowiedniego pola kategorii dla każdego produktu (takie jak `CategoryName`).

> [!NOTE]
> `JOIN` s są często używane podczas wykonywania zapytań dotyczących danych z relacyjnej bazy danych. Jeśli dopiero zaczynasz korzystać z `JOIN` składni lub trzeba nieco projektujesz jej użycie d zalecam [samouczek SQL Join](http://www.w3schools.com/sql/sql_join.asp) na [szkół W3](http://www.w3schools.com/). Warto odczytu są także [ `JOIN` podstawy](https://msdn.microsoft.com/library/ms191517.aspx) i [podstawy podzapytania](https://msdn.microsoft.com/library/ms189575.aspx) sekcje [SQL — książki Online](https://msdn.microsoft.com/library/ms130214.aspx).


Ponieważ `JOIN` s i skorelowane podzapytania mogą posłużyć do pobierania danych powiązane z innymi tabelami, wielu deweloperów są pozostawiane drapanie ich nagłówki i zastanawiasz się, które rozwiązanie do użycia. Wszystkie SQL gurus I ve Rozmawialiśmy do dostęp mniej więcej tak samo, że t ma znaczenia performance-wise jako programu SQL Server powoduje wygenerowanie plany wykonywania mniej więcej taka sama. Porady, następnie polega na użyciu techniki, które Ty i Twój zespół są najbardziej Ci. Uzasadnia, biorąc pod uwagę, że po nadając tej porady te ekspertów natychmiast express swoje preferencje `JOIN` s za pośrednictwem skorelowany podzapytania.

Podczas tworzenia warstwy dostępu do danych, przy użyciu wpisanych zestawów danych, narzędzia działają lepiej korzystając z podzapytania. W szczególności, Kreator s TableAdapter nie generowane automatycznie odpowiadającego `INSERT`, `UPDATE`, i `DELETE` instrukcji, jeśli główne zapytanie zawiera którykolwiek `JOIN` s, ale generowane automatycznie tych instrukcji, gdy skorelowane podzapytania są używane.

Aby poznać tego braku, Utwórz tymczasowe wpisana zestawu danych w `~/App_Code/DAL` folderu. W Kreatorze konfiguracji TableAdapter chce używać instrukcji języka SQL zapytań ad-hoc, a następnie wprowadź następujące `SELECT` zapytania (patrz rysunek 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Wprowadź główne zapytanie, która zawiera sprzężenia](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Rysunek 1**: Wprowadź kwerendę Main zawierający `JOIN` s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


Domyślnie automatycznie utworzy TableAdapter `INSERT`, `UPDATE`, i `DELETE` instrukcji na podstawie głównego zapytania. Po kliknięciu przycisku Zaawansowane widać, że ta funkcja jest włączona. Niezależnie od tego ustawienia TableAdapter nie będzie mogła tworzyć `INSERT`, `UPDATE`, i `DELETE` instrukcji ponieważ główne zapytanie zawiera `JOIN`.


![Wprowadź główne zapytanie, która zawiera sprzężenia](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Rysunek 2**: Wprowadź główne zapytanie, która zawiera `JOIN` s


Kliknij przycisk Zakończ, aby zakończyć działanie kreatora. W tym momencie usługi s DataSet Designer będzie zawierać pojedynczy obiekt TableAdapter z obiektu DataTable z kolumny dla każdego pola, zwracany w `SELECT` listy kolumny zapytania s. Obejmuje to `CategoryName` i `SupplierName`, jak pokazano na rysunku 3.


![DataTable zawiera kolumnę dla każdego pola, zwrócony na liście kolumn](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Rysunek 3**: DataTable zawiera kolumnę dla każdego pola zwrócił się na liście kolumn


Chociaż DataTable ma odpowiednie kolumny, TableAdapter nie ma wartości dla swojej `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości. Można to potwierdzić, kliknij na obiekt TableAdapter w projektancie, a następnie przejdź do okna właściwości. Będzie informacja, że `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości są ustawione na (Brak).


[![Element InsertCommand elementu UpdateCommand i właściwości elementu DeleteCommand są ustawione na (Brak)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Rysunek 4**: `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości są ustawione na (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


W celu obejścia tego braku, można ręcznie zapewniamy instrukcji SQL i parametry `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości, za pośrednictwem oknie właściwości. Alternatywnie można Zaczniemy od konfiguracji zapytania TableAdapter głównego s, aby *nie* zawierać `JOIN` s. Dzięki temu będzie `INSERT`, `UPDATE`, i `DELETE` instrukcje, aby być generowane automatycznie dla nas. Po ukończeniu kreatora, firma Microsoft może następnie ręcznie zaktualizować TableAdapter s `SelectCommand` w oknie właściwości, tak że obejmuje `JOIN` składni.

Chociaż to podejście pozwala skutecznie, jest bardzo kruchy po przy użyciu zapytań SQL w trybie ad-hoc, ponieważ w dowolnym momencie główne zapytanie TableAdapter s ponownie skonfigurować za pomocą Kreatora automatycznego generowania `INSERT`, `UPDATE`, i `DELETE` instrukcje są tworzone ponownie. Oznacza to, że wszystkie modyfikacje później wprowadziliśmy zostałyby utracone jeśli możemy kliknięto prawym przyciskiem myszy na obiekt TableAdapter, wybrana konfiguracja z menu kontekstowego i ukończyć kreatora ponownie.

Kruchości s TableAdapter generowanych automatycznie `INSERT`, `UPDATE`, i `DELETE` instrukcji jest można jednak ograniczone do instrukcji SQL zapytań ad-hoc. Twoje TableAdapter używa procedur składowanych, można dostosować `SelectCommand`, `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` procedur składowanych i ponownie uruchom Kreator konfiguracji TableAdapter bez obawy, że będzie procedur składowanych zmodyfikowane.

Przez następne kilka kroków, które utworzymy TableAdapter, która początkowo używa główne zapytanie, które pomija dowolne `JOIN` s, aby odpowiednie wstawiania, aktualizacji i usuwania przechowywane procedury będą generowane automatycznie. Następnie zaktualizujemy `SelectCommand` tak, który używa `JOIN` zwracającego dodatkowe kolumny z powiązanych tabel. Na koniec utworzymy odpowiednią klasę warstwy logiki biznesowej i demonstrującą używanie TableAdapter strony sieci web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Krok 1: Tworzenie TableAdapter za pomocą uproszczonego główne zapytanie

W tym samouczku dodamy TableAdapter i silnie typizowaną klasę DataTable `Employees` tabelę `NorthwindWithSprocs` zestawu danych. `Employees` Tabela zawiera `ReportsTo` pola, które określone `EmployeeID` menedżera pracownika s. Na przykład pracownik ma Dodsworth Anna `ReportTo` wartość 5, która jest `EmployeeID` z Bator Steven. W związku z tym Anna raporty Steven, swojemu menedżerowi. Wraz z raportowania każdemu pracownikowi s `ReportsTo` wartości, firma Microsoft może być również konieczne pobrać nazwy jego menedżera. Można to zrobić za pomocą `JOIN`. Jednak przy użyciu `JOIN` podczas początkowo tworzenie TableAdapter wyklucza kreatora z automatycznego generowania odpowiednich wstawiania, aktualizowania i usuwania możliwości. W związku z tym, Zaczniemy, tworząc TableAdapter, w których główne zapytanie nie zawiera żadnych `JOIN` s. Następnie w kroku 2, zaktualizujemy procedurę główne zapytanie przechowywane, aby pobrać nazwę menedżera s za pośrednictwem `JOIN`.

Zacznij od otwarcia `NorthwindWithSprocs` zestawu danych w `~/App_Code/DAL` folderu. Kliknij prawym przyciskiem myszy w projektancie, wybierz opcję Dodaj menu kontekstowe i wybierz element menu TableAdapter. Spowoduje to uruchomienie Kreatora konfiguracji TableAdapter. Jak przedstawiono na rysunku 5, należy za pomocą kreatora, Utwórz nowe procedury składowane, a następnie kliknij przycisk Dalej. Dla przypomnienia informacji na temat tworzenia nowych procedur składowanych z Kreatora s TableAdapter, zapoznaj się [tworzenie nowych procedur składowanych dla s wpisany zestaw danych TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka.


[![Wybierz pozycję Utwórz nowe procedury składowane opcji](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Rysunek 5**: wybierz pozycję Utwórz nowe procedury składowane opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Należy użyć następującego `SELECT` instrukcji kwerendy głównych s TableAdapter:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Ponieważ to zapytanie nie zawiera żadnych `JOIN` s, TableAdapter Kreator automatycznie utworzy procedur składowanych z odpowiednimi `INSERT`, `UPDATE`, i `DELETE` instrukcji, a także procedury składowanej do wykonywania głównych Zapytanie.

Następny krok pozwala nam nazwę procedury przechowywane s TableAdapter. Użyj nazw `Employees_Select`, `Employees_Insert`, `Employees_Update`, i `Employees_Delete`, jak pokazano na rysunku 6.


[![Nazwa procedury przechowywane s TableAdapter](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Rysunek 6**: Nazwa procedur składowanych s TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


Ostatnim krokiem monituje o nas nazwę metody s TableAdapter. Użyj `Fill` i `GetEmployees` jako nazwy metody. Należy także pozostawić Utwórz metody służące do wysyłania aktualizacji bezpośrednio do bazy danych (GenerateDBDirectMethods) zaznacz pole wyboru zaznaczone.


[![Nazwa metody wypełnienie s TableAdapter i GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Rysunek 7**: Nazwa s TableAdapter metody `Fill` i `GetEmployees` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Po ukończeniu kreatora, Poświęć chwilę, aby zbadać procedur składowanych w bazie danych. Powinny zostać wyświetlone cztery nowe: `Employees_Select`, `Employees_Insert`, `Employees_Update`, i `Employees_Delete`. Następnie sprawdź `EmployeesDataTable` i `EmployeesTableAdapter` właśnie utworzony. DataTable zawiera kolumnę dla każdego pola, zwrócony przez główne zapytanie. Kliknij w metodzie TableAdapter, a następnie przejdź do okna właściwości. Będzie informacja, że `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości są prawidłowo skonfigurowane do wywoływania odpowiednich procedur składowanych.


[![TableAdapter obejmuje Insert, Update i usuwanie funkcji](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Rysunek 8**: TableAdapter obejmuje Insert, Update i usuwanie możliwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


Za pomocą wstawiania, aktualizowania i usuwania automatycznie tworzyć procedury składowane i `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości poprawnie skonfigurowana, możemy przystąpić do dostosowywania `SelectCommand` s przechowywane procedury, aby zwrócić dodatkowe informacje na temat każdego menedżera pracownika s. W szczególności należy zaktualizować `Employees_Select` użyj procedury składowanej `JOIN` i zwracają Menedżera s `FirstName` i `LastName` wartości. Po zaktualizowaniu procedury składowanej będzie musimy zaktualizować na obiekt DataTable, aby obejmowała te dodatkowe kolumny. Firma Microsoft będzie czoła te dwa zadania w krokach 2 i 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Krok 2: Dostosowywanie procedury składowanej do uwzględnienia`JOIN`

Początek, przechodząc do Eksploratora serwera, przechodzenie do szczegółów w folderze procedur składowanych s bazy danych Northwind i otwierania `Employees_Select` procedury składowanej. Jeśli nie widzisz tej procedury składowanej, kliknij prawym przyciskiem myszy w folderze procedur składowanych, a następnie wybierz polecenie Odśwież. Zaktualizuj procedury składowanej, tak aby używał `LEFT JOIN` najpierw zwrócić Menedżera s i nazwisko:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Po zaktualizowaniu `SELECT` instrukcji, Zapisz zmiany, przechodząc do menu Plik i wybierając pozycję Zapisz `Employees_Select`. Alternatywnie możesz kliknij ikonę Zapisz na pasku narzędzi lub naciśnij klawisze Ctrl + S. Po zapisaniu zmian, kliknij prawym przyciskiem myszy `Employees_Select` procedurę składowaną w Eksploratorze serwera i wybierz polecenie Execute. Spowoduje to uruchomienie procedury składowanej i wyświetlić wyniki w oknie danych wyjściowych (patrz rysunek 9).


[![Wyniki procedury przechowywane są wyświetlane w oknie danych wyjściowych](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Rysunek 9**: przechowywane procedury wyniki są wyświetlane w oknie danych wyjściowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Krok 3: Aktualizowanie kolumn s DataTable

W tym momencie `Employees_Select` przechowywane procedury zwraca `ManagerFirstName` i `ManagerLastName` wartości, ale `EmployeesDataTable` brakuje tych kolumn. Można dodać te Brak kolumn do DataTable w jeden z dwóch sposobów:

- **Ręcznie** — kliknij prawym przyciskiem myszy na DataTable w Projektancie obiektów DataSet i z menu Dodaj, wybierz kolumnę. Można następnie nazwa kolumny i odpowiednio ustawić jej właściwości.
- **Automatycznie** — Kreator konfiguracji TableAdapter zaktualizuje kolumn tabeli DataTable s, aby odzwierciedlić pola, zwracany przez `SelectCommand` procedury składowanej. Korzystając z instrukcji SQL zapytań ad-hoc, Kreator spowoduje również usunięcie `InsertCommand`, `UpdateCommand`, i `DeleteCommand` właściwości od `SelectCommand` zawiera teraz `JOIN`. Jednak korzystając z procedur składowanych, te właściwości polecenia pozostają bez zmian.

Rozważyliśmy ręcznego dodawania kolumn tabeli DataTable w poprzednich samouczkach, w tym [Master/szczegółów przy użyciu punktowanej listy z rekordów głównych z kontrolką DataList szczegółów](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) i [przesyłanie plików](../working-with-binary-files/uploading-files-cs.md), a firma Microsoft będzie Obejrzyj ten proces ponownie bardziej szczegółowo w naszym samouczku dalej. W tym samouczku jednak umożliwia automatyczne podejście za pomocą Kreatora konfiguracji TableAdapter s.

Rozpocznij, klikając prawym przyciskiem myszy `EmployeesTableAdapter` i wybierając pozycję Konfiguruj, z menu kontekstowego. Otwarte Kreatora konfiguracji TableAdapter, który zawiera listę procedur składowanych, umożliwiający wybranie, wstawianie, aktualizowanie i usuwanie wraz z ich wartości zwracane i parametry (jeśli istnieje). Na rysunku nr 10 przedstawiono tego kreatora. W tym miejscu można widzimy, że `Employees_Select` przechowywane procedury teraz zwraca `ManagerFirstName` i `ManagerLastName` pola.


[![Kreator pokazuje listy kolumn zaktualizowane dla Employees_Select procedury składowanej](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Na rysunku nr 10**: Kreator pokazuje listy kolumn zaktualizowane dla `Employees_Select` Stored Procedure ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Ukończ pracę kreatora, klikając przycisk Zakończ. Po powrocie do Projektanta obiektów DataSet `EmployeesDataTable` zawiera dwa dodatkowe kolumny: `ManagerFirstName` i `ManagerLastName`.


[![EmployeesDataTable zawiera dwie nowe kolumny](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Rysunek 11**: `EmployeesDataTable` zawiera dwie nowe kolumny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Aby zilustrować, że zaktualizowany `Employees_Select` procedury składowanej jest aktywna i czy wstawiania, aktualizowania i usuwania możliwości TableAdapter działają nadal, niech s, Utwórz stronę sieci web, która umożliwia użytkownikom wyświetlanie i usuwanie pracowników. Przed utworzeniem taka strona, jednak należy najpierw utworzyć nową klasę w warstwy logiki biznesowej do pracy z pracownikami z `NorthwindWithSprocs` zestawu danych. W kroku 4, utworzymy `EmployeesBLLWithSprocs` klasy. W kroku 5 użyjemy tej klasy ze strony programu ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Krok 4: Wdrażanie warstwy logiki biznesowej

Utwórz nowy plik klasy w `~/App_Code/BLL` folder o nazwie `EmployeesBLLWithSprocs.cs`. Ta klasa naśladuje semantyki istniejącej `EmployeesBLL` klasy, tylko tym nowe jeden udostępnia metody mniej i używa `NorthwindWithSprocs` zestawu danych (zamiast `Northwind` zestawu danych). Dodaj następujący kod do `EmployeesBLLWithSprocs` klasy.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

`EmployeesBLLWithSprocs` Klasy s `Adapter` właściwość zwraca wystąpienie `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Jest on używany przez klasy s `GetEmployees` i `DeleteEmployee` metody. `GetEmployees` Wywołania metody `EmployeesTableAdapter` odpowiadającego s `GetEmployees` metody, która wywołuje `Employees_Select` procedury składowanej i wypełnia jego wyniki w `EmployeeDataTable`. `DeleteEmployee` Podobnie wywołania metod `EmployeesTableAdapter` s `Delete` metody, która wywołuje `Employees_Delete` procedury składowanej.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Krok 5: Praca z danymi w warstwie prezentacji

Za pomocą `EmployeesBLLWithSprocs` klasy kompletne, możemy ponownie gotowe do pracy z danymi pracowników przy użyciu strony ASP.NET. Otwórz `JOINs.aspx` strony w `AdvancedDAL` folder i przeciągnij GridView z przybornika w projektancie, ustawiając jego `ID` właściwość `Employees`. Następnie za pomocą tagu inteligentnego s GridView powiązać siatki nowej kontrolki ObjectDataSource o nazwie `EmployeesDataSource`.

Konfigurowanie kontrolki ObjectDataSource używać `EmployeesBLLWithSprocs` klasy, a następnie z karty Wybierz i Usuń, upewnij się, że `GetEmployees` i `DeleteEmployee` metody są wybrane z listy rozwijanej. Kliknij przycisk Zakończ, aby zakończyć konfigurowanie s ObjectDataSource.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Rysunek 12**: Konfigurowanie kontrolki ObjectDataSource do użycia `EmployeesBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![Mogą używać kontrolki ObjectDataSource GetEmployees i metod DeleteEmployee](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Rysunek 13**: mogą używać kontrolki ObjectDataSource `GetEmployees` i `DeleteEmployee` metody ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Program Visual Studio spowoduje to dodanie elementu BoundField do kontrolki GridView dla każdego z `EmployeesDataTable` s kolumn. Usuń wszystkie te BoundFields, z wyjątkiem `Title`, `LastName`, `FirstName`, `ManagerFirstName`, i `ManagerLastName` i Zmień nazwę `HeaderText` właściwości dla ostatnich czterech BoundFields Last Name, imię, imię Menedżera s i Menedżer s nazwisko, odpowiednio.

Aby zezwolić użytkownikom na usuwanie pracowników z tej strony, musimy wykonać dwie czynności. Po pierwsze poinstruuj GridView zapewnienie możliwości usuwania, zaznaczając opcję Włącz usuwanie, z jego tagu inteligentnego. Po drugie, zmiana ObjectDataSource s `OldValuesParameterFormatString` z wartością ustawioną przez kreatora kontrolki ObjectDataSource (`original_{0}`) do wartości domyślnej (`{0}`). Po wprowadzeniu tych zmian, z kontrolkami GridView i kontrolki ObjectDataSource s oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Przetestowania strony, odwiedzając za pośrednictwem przeglądarki. Jak pokazano na rysunku 14, strony wyświetli listę każdego pracownika i Menedżera s imienia (przy założeniu, że mają one).


[![SPRZĘŻENIA w Employees_Select przechowywane procedury zwraca nazwę s Menedżera](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Rysunek 14**: `JOIN` w `Employees_Select` procedury składowanej zwraca Menedżera s Nazwa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


Kliknięcie przycisku Usuń rozpoczyna się usuwanie przepływu pracy, którą culminates w trakcie wykonania `Employees_Delete` procedury składowanej. Jednak próba dokonania `DELETE` instrukcji w procedurze składowanej zakończy się niepowodzeniem z powodu naruszenia ograniczenia klucza obcego (zobacz rysunek 15). W szczególności każdy pracownik ma co najmniej jednego rekordu `Orders` tabeli, co powoduje usunięcie nie powiedzie się.


[![Trwa usuwanie pracownika, który ma odpowiadające im wyniki zamówienia w naruszenie ograniczenia klucza obcego](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Rysunek 15**: usuwanie pracownika, który ma odpowiadające im wyniki zamówienia w naruszenie ograniczenia klucza obcego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Umożliwia pracownika usunięty użytkownik może:

- Aktualizacja kaskadowo usuwa, ograniczenie klucza obcego
- Ręcznie usuń rekordy z `Orders` tabeli dla pracowników do usunięcia, lub
- Aktualizacja `Employees_Delete` najpierw usuń powiązane rekordy z procedury składowanej `Orders` tabeli przed usunięciem `Employees` rekordu. Omówiliśmy tej techniki w [za pomocą istniejących procedur składowanych dla s wpisany zestaw danych TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) samouczka.

Można pozostawić to w charakterze ćwiczenia dla czytnika.

## <a name="summary"></a>Podsumowanie

Podczas pracy z relacyjnych baz danych, często zapytania ich dane z wielu powiązanych tabelach. Skorelowane podzapytań i `JOIN` s Podaj dwie techniki uzyskiwania dostępu do danych z powiązanych tabel w zapytaniu. W poprzednich samouczkach najczęściej wprowadziliśmy użytkowania skorelowany podzapytań ponieważ TableAdapter nie może automatycznie wygenerować `INSERT`, `UPDATE`, i `DELETE` instrukcji zapytań obejmujących `JOIN` s. Gdy te wartości można podać ręcznie, korzystając z instrukcji SQL zapytań ad-hoc, wszelkie dostosowania zostaną zastąpione po ukończeniu działania Kreatora konfiguracji TableAdapter.

Na szczęście TableAdapters utworzone za pomocą procedur składowanych nie odczuwają kruchości ten sam, jak te utworzone za pomocą instrukcji SQL zapytań ad-hoc. Dlatego jest to możliwe, aby utworzyć obiekt TableAdapter używa których główne zapytanie `JOIN` korzystając z procedur składowanych. W tym samouczku będziemy pokazaliśmy, jak utworzyć obiekt TableAdapter. Rozpoczęliśmy przy użyciu `JOIN`— mniej `SELECT` wyszukiwać TableAdapter s główne zapytanie tak, że odpowiednie insert, update i delete, przechowywane procedury utworzony automatycznie. Za pomocą TableAdapter s konfiguracji początkowej pełną, firma Microsoft rozszerzone `SelectCommand` użyj procedury składowanej `JOIN` i ponownie uruchomiono kreatora konfiguracji TableAdapter w celu zaktualizowania `EmployeesDataTable` s kolumn.

Ponowne uruchomienie Kreatora konfiguracji TableAdapter, automatycznie aktualizowane `EmployeesDataTable` kolumny odzwierciedla pola danych zwracane przez `Employees_Select` procedury składowanej. Alternatywnie można dodaliśmy te kolumny ręcznie do elementu DataTable. Przeanalizujemy ręczne dodawanie kolumn do DataTable w następnym samouczku.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Hilton Geisenow, David Suru i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [dalej](adding-additional-datatable-columns-cs.md)
