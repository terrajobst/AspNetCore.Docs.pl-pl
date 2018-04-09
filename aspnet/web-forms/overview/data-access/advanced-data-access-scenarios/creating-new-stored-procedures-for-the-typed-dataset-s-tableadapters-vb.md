---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Tworzenie nowego procedur składowanych do TableAdapters Typizowanego obiektu DataSet (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W samouczkach wcześniejszych możemy utworzeniu instrukcji SQL w naszym kodzie i przekazany instrukcje do bazy danych do wykonania. Alternatywną metodą jest s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 3df3623f5575a48a22fb1a2c3bc719975a80b9c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Tworzenie nowego procedur składowanych do TableAdapters Typizowanego obiektu DataSet (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) lub [pobierania plików PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> W samouczkach wcześniejszych możemy utworzeniu instrukcji SQL w naszym kodzie i przekazany instrukcje do bazy danych do wykonania. Informacje o innym podejściu jest używane procedury składowane, w którym instrukcje SQL są wstępnie zdefiniowane w bazie danych. W tym samouczku będziemy Dowiedz się, jak Kreator TableAdapter wygenerować nowe procedury składowane w firmie Microsoft.


## <a name="introduction"></a>Wprowadzenie

Te samouczki warstwy dostępu do danych (DAL) używa wpisanych zestawów danych. Zgodnie z opisem w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczka Typed zestawów danych składa się z jednoznacznie DataTables i TableAdapters. DataTables reprezentują jednostek logicznych w systemie podczas interfejsu TableAdapters z podstawowej bazy danych do wykonywania pracy dostępu do danych. W tym wypełnianie DataTables z danymi, wykonywanie zapytań, które zwracają dane skalarne, wstawiania, aktualizowania i usuwania rekordów z bazy danych.

Polecenia SQL wykonywane przez TableAdapters może być albo instrukcji SQL ad-hoc, takich jak `SELECT columnList FROM TableName`, lub procedur składowanych. TableAdapters w naszej architektury użyj instrukcji SQL ad hoc. Wiele deweloperom i administratorom bazy danych, jednak preferować procedury składowane za pośrednictwem instrukcji SQL ad hoc ze względów bezpieczeństwa, łatwość utrzymania, a także czy można je aktualizować. Inne ardently preferowane ad hoc instrukcji SQL dla ich elastyczność. Własne pracy I Preferuj procedury składowane za pośrednictwem instrukcji SQL ad hoc, ale postanowiono upraszczanie samouczki wcześniej przy użyciu instrukcji SQL ad hoc.

Podczas definiowania TableAdapter lub dodawania nowych metod, Kreator s TableAdapter ułatwia równie łatwo utworzyć nowe procedury składowane lub użyć istniejących procedur składowanych, jak używać instrukcji SQL ad hoc. W tym samouczku zajmiemy się, jak s TableAdapter Kreator automatycznego generowania procedur składowanych. W następnym samouczku przedstawiono sposób konfigurowania metody s TableAdapter istniejących lub ręcznie utworzone procedur składowanych.

> [!NOTE]
> Zobacz wpis w blogu Howard Tomasz [ADAM t Użyj przechowywanych procedur jeszcze?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) i [Frans Bouma](https://weblogs.asp.net/fbouma/) wpis w blogu s [procedury składowane są nieprawidłowe, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) dla barwnym dyskusji na zalet i wad procedury składowane i SQL ad hoc.


## <a name="stored-procedure-basics"></a>Podstawowe informacje dotyczące procedury składowanej

Funkcje są wspólne dla wszystkich języków programowania konstrukcję. Funkcja to zbiór instrukcji, które są wykonywane, gdy funkcja jest wywoływana. Funkcje może akceptować parametry wejściowe i opcjonalnie może zwracać wartości. *[Procedury składowane](http://en.wikipedia.org/wiki/Stored_procedure)*  są konstrukcji bazy danych, które mają wiele podobieństw z funkcjami w językach programowania. Procedura składowana składa się z zestawem instrukcje T-SQL, które są wykonywane, gdy jest wywoływana procedura składowana. Procedura składowana może przyjmować wartości zero spowoduje wiele parametrów wejściowych i może zwrócić wartości skalarnych, parametry wyjściowe lub najczęściej zestawy wyników z `SELECT` zapytania.

> [!NOTE]
> Procedury składowane są często określane jako sprocs lub SPs.


Procedury składowane są tworzone przy użyciu [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) instrukcji T-SQL. Na przykład poniższy skrypt T-SQL tworzy procedury składowanej o nazwie `GetProductsByCategoryID` który przyjmuje jeden parametr o nazwie `@CategoryID` i zwraca `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` pola tych kolumn `Products` tabeli, która ma zgodnego `CategoryID` wartość:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Po utworzeniu tej procedury składowanej, można wywołać, używając następującej składni:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> W następnym samouczku omówione zostanie Tworzenie procedur składowanych za pomocą środowiska IDE programu Visual Studio. W tym samouczku jednak zamierzamy let TableAdapter Kreator automatycznego generowania procedur składowanych w firmie Microsoft.


Oprócz po prostu zwrócenie danych, procedury składowane są często używane do wykonywania wielu poleceń bazy danych w zakresie jednej transakcji. Procedura składowana o nazwie `DeleteCategory`, na przykład może zająć `@CategoryID` parametru i wykonania dwóch `DELETE` instrukcje:, pierwszego w celu usunięcia pokrewnych produktów i drugi, usunięcie określonej kategorii. Użycie wielu instrukcji w procedurze składowanej są *nie* automatycznie zawijany w obrębie transakcji. Dodatkowe polecenia T-SQL muszą zostanie wysłane w celu zapewnienia procedury składowanej s wielu poleceń są traktowane jako operacją niepodzielną. Będzie przedstawiono sposób zawijania poleceń s procedury przechowywanej w zakresie transakcji kolejne kroki samouczka.

Korzystając z procedur składowanych w ramach architektury, metody s Warstwa dostępu do danych wywołania określonej procedury składowanej niż wystawianie instrukcji SQL ad hoc. Umożliwia to scentralizowanie lokalizacji instrukcji SQL wykonane (na bazie danych) zamiast on zdefiniowany w ramach architektura s. Takie scentralizowane podejście raczej ułatwia znajdowanie, analizowanie i dostrajania zapytań i udostępnia wiele jaśniejszy obraz określające, jak i gdzie baza danych jest używana.

Aby uzyskać więcej informacji na podstawowe informacje dotyczące procedury składowanej zapoznaj się z zasobów w sekcji dalsze informacje na końcu tego samouczka.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Krok 1: Tworzenie stron sieci Web scenariusze warstwy dostępu do danych zaawansowane

Przed Rozpoczniemy naszych dyskusji dotyczących tworzenia DAL, korzystanie z procedur składowanych umożliwiają s najpierw Poświęć chwilę, aby utworzyć w naszym projekt witryny sieci Web, które są wymagane dla tego i dalej samouczki kilka stron ASP.NET. Rozpocznij od dodania nowy folder o nazwie `AdvancedDAL`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Dodawanie stron ASP.NET samouczki scenariusze warstwy dostępu do danych zaawansowane](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Rysunek 1**: Dodawanie stron ASP.NET samouczki scenariusze warstwy dostępu do danych zaawansowane


Podobnie jak w innych folderach `Default.aspx` w `AdvancedDAL` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


Na koniec Dodaj te strony jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po zakończeniu pracy z danymi umieścić w zadaniu wsadowym `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy zaawansowane samouczki scenariusze DAL.


![Mapy witryny zawiera teraz wpisy samouczki scenariusze DAL zaawansowane](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Rysunek 3**: mapy witryny zawiera teraz wpisy samouczki scenariusze DAL zaawansowane


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Krok 2: Konfigurowanie TableAdapter, aby utworzyć nowe procedury składowane

Aby zademonstrować, tworzenie Warstwa dostępu do danych, która używa procedur składowanych zamiast instrukcji SQL ad-hoc, umożliwiają s utworzyć nowe wpisane zestawu danych w `~/App_Code/DAL` folder o nazwie `NorthwindWithSprocs.xsd`. Ponieważ firma Microsoft udzieleniu ten proces szczegółowo w poprzednim samouczki, możemy przejdą szybko kroków w tym miejscu. Jeśli zatrzymywane lub konieczne dalsze instrukcje krok po kroku w tworzenie i konfigurowanie wpisane zestawu danych, odwołaj się do [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczka.

Dodaj nowy zestaw danych do projektu, klikając prawym przyciskiem myszy `DAL` folderu, wybierając pozycję Dodaj nowy element, a wybór szablonu zestawu danych, jak pokazano na rysunku 4.


[![Dodaj nowy Typizowany zestaw danych do projektu o nazwie NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Rysunek 4**: Dodaj nowy zestaw danych o typie określonym do projektu o nazwie `NorthwindWithSprocs.xsd` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Spowoduje to utworzenie nowy zestaw danych wpisane, otwórz jego projektanta, Utwórz nowy obiekt TableAdapter i uruchom Kreatora konfiguracji TableAdapter. Pierwszym krokiem s TableAdapter Kreator konfiguracji zapyta nam wybierz bazę danych do pracy z. Parametry połączenia z bazą danych Northwind powinien być wyświetlany na liście rozwijanej. Wybierz tę opcję i kliknij przycisk Dalej.

Na tym ekranie dalej wybieramy opcję jak TableAdapter powinien uzyskiwać dostęp do bazy danych. W poprzednich samouczki Wybraliśmy pierwsza opcja, instrukcje SQL do użycia. W tym samouczku wybierz opcję, drugi, Utwórz nowe procedury składowane i kliknij przycisk Dalej.


[![Poinstruuj TableAdpater, aby utworzyć nowe procedury składowane](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Rysunek 5**: poinstruować TableAdpater, aby utworzyć nowe procedury składowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Tak samo jak z wykorzystaniem instrukcji SQL ad-hoc, w następnym kroku będziemy są proszeni o dostarczenie `SELECT` instrukcji kwerendy głównego s TableAdapter. Zamiast używać, ale `SELECT` instrukcji wprowadzanym w tym miejscu do wykonywania zapytań ad hoc bezpośrednio, TableAdapter s Kreator utworzy procedury przechowywanej, która zawiera to `SELECT` zapytania.

Należy użyć następującego `SELECT` zapytania dla tej TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![Wprowadź zapytania SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Rysunek 6**: wprowadź `SELECT` kwerendy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> Powyższe zapytanie różni się nieco od głównego zapytania `ProductsTableAdapter` w `Northwind` wpisane zestawu danych. Odwołania, który `ProductsTableAdapter` w `Northwind` wpisane zestaw danych zawiera dwa skorelowane podzapytań, aby przywrócić nazwy kategorii i nazwę firmy dla każdej kategorii produktów s i dostawcy. W kolejnych [aktualizowanie TableAdapter w celu użycia sprzężenia](updating-the-tableadapter-to-use-joins-vb.md) dane dotyczące zajmiemy Dodawanie tego samouczka do tego TableAdapter.


Poświęć chwilę, kliknij przycisk Opcje zaawansowane. W tym miejscu można określić czy kreator powinien także wygenerować insert, update i delete instrukcje TableAdapter, czy użyć optymistycznej współbieżności i określa, czy tabela danych powinny być odświeżane po instrukcji INSERT i Update. Domyślnie jest zaznaczona opcja instrukcje generowania Insert, Update i Delete. Pozostaw zaznaczone. W tym samouczku nie zaznaczaj wyboru Użyj opcji optymistycznej współbieżności.

Gdy ma procedur składowanych automatycznie tworzone przez kreatora TableAdapter, wygląda na to, że odświeżania danych opcji tabeli jest ignorowane. Niezależnie od tego, czy to pole wyboru jest zaznaczone pole wyboru, wynikowy insert i update procedur składowanych pobrać rekordu just wstawiane lub aktualizowane po prostu, zostanie wyświetlone w kroku 3.


![Pozostaw instrukcje generowania Insert, Update i Delete, zaznaczone pole](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Rysunek 7**: pozostaw instrukcje generowania Insert, Update i Delete, zaznaczone pole


> [!NOTE]
> Jeśli zaznaczono opcję Użyj optymistycznej współbieżności, Kreator doda dodatkowe warunki `WHERE` klauzuli uniemożliwić aktualizację, jeśli wprowadzono zmian w innych pól danych. Odwołaj się do [implementacja optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) samouczka, aby uzyskać więcej informacji na temat używania funkcji Kontrola TableAdapter s wbudowanych optymistycznej współbieżności.


Po wprowadzeniu `SELECT` zapytań i potwierdzenie, że jest zaznaczona opcja instrukcje generowania Insert, Update i Delete, kliknij przycisk Dalej. Ten ekran dalej, pokazano na rysunku 8 monituje o podanie nazwy procedur składowanych, który Kreator utworzy zaznaczenie, wstawianie, aktualizowanie i usuwanie danych. Zmiana tych przechowywanych procedur nazwy do `Products_Select`, `Products_Insert`, `Products_Update`, i `Products_Delete`.


[![Zmień nazwę procedury składowane](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Rysunek 8**: Zmień nazwę procedury składowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


Aby wyświetlić TableAdapter kreator użyje do utworzenia procedur składowanych cztery T-SQL, kliknij przycisk skrypt SQL w wersji zapoznawczej. W oknie dialogowym Podgląd skryptu SQL może zapisać do pliku skryptu lub skopiuj go do Schowka.


![Wyświetl podgląd skryptu SQL używanego do wygenerowania procedur składowanych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Rysunek 9**: Podgląd skryptu SQL używanego do generowania procedur składowanych


Po nazw procedur składowanych, kliknij przycisk Dalej, nazwa TableAdapter s odpowiednich metod. Podobnie jak przy użyciu instrukcji SQL ad-hoc można utworzyć metody wypełnienia istniejącego elementu DataTable lub zwraca nową. Możemy również określić, czy TableAdapter powinien zawierać wzorca bazy danych bezpośrednio do wstawiania, aktualizowania i usuwania rekordów. Pozostaw zaznaczone wszystkie trzy pola wyboru, ale zmienić zwracany metodę DataTable `GetProducts` (jak pokazano na rysunku nr 10).


[![Nazwa metody Fill i GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Na rysunku nr 10**: Nazwa metody `Fill` i `GetProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Kliknij przycisk Dalej, aby wyświetlić podsumowanie czynności, które wykona Kreator. Ukończ pracę kreatora, klikając przycisk Zakończ. Po zakończeniu działania kreatora nastąpi powrót do zestawu danych s projektanta, który teraz obejmuje `ProductsDataTable`.


[![Projektant obiektów DataSet s pokazuje nowo dodanego ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Rysunek 11**: s zestawu danych projektanta pokazuje nowo dodanych `ProductsDataTable` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Krok 3: Badanie nowo utworzony procedury składowane

TableAdapter Kreator używany w kroku 2 automatycznie utworzyć procedur składowanych zaznaczenie, wstawianie, aktualizowanie i usuwanie danych. Te procedury składowane można wyświetlić lub zmodyfikować za pomocą programu Visual Studio, przechodząc do Eksploratora serwera i przechodzenie do folderu bazy danych s procedur składowanych. Jak pokazano na rysunku 12, bazy danych Northwind zawiera cztery nowe procedury składowane: `Products_Delete`, `Products_Insert`, `Products_Select`, i `Products_Update`.


![Cztery utworzony w kroku 2 procedury składowane można znaleźć w folderze procedury przechowywane bazy danych s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Rysunek 12**: cztery utworzony w kroku 2 procedury składowane można znaleźć w folderze procedury przechowywane bazy danych s


> [!NOTE]
> Jeśli nie ma Eksploratora serwera, przejdź do menu Widok i wybierz opcję Eksploratora serwera. Jeśli związane z produktem procedur składowanych dodane z kroku nr 2 nie są wyświetlane, spróbuj prawym przyciskiem myszy folder na procedury składowane i wybierając polecenie Odśwież.


Aby wyświetlić lub zmodyfikować procedury składowanej, kliknij dwukrotnie jego nazwę w Eksploratorze serwera, lub Alternatywnie kliknij prawym przyciskiem procedury składowanej i wybierz polecenie Otwórz. Przedstawia rysunek 13 `Products_Delete` przechowywane procedury, po otwarciu.


[![Procedury składowane można otworzyć i modyfikować z poziomu programu Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Rysunek 13**: przechowywane procedury mogą być otwarte i zmodyfikowane z w Visual Studio ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


Zawartość zarówno `Products_Delete` i `Products_Select` procedury składowane są bardzo proste. `Products_Insert` i `Products_Update` procedur składowanych z drugiej strony, gwarantuje ściślejszej kontroli wykonujących zarówno `SELECT` instrukcji po ich `INSERT` i `UPDATE` instrukcje. Na przykład następujące instrukcje SQL stanowi `Products_Insert` procedury składowanej:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Procedura składowana przyjmuje jako parametry wejściowe `Products` kolumny, które zostały zwrócone przez `SELECT` zapytanie określone w Kreatorze s TableAdapter i te wartości są używane w `INSERT` instrukcji. Po `INSERT` instrukcji, `SELECT` zapytanie służy do zwracania `Products` wartości w kolumnie (w tym `ProductID`) nowo dodanego rekordu. Ta funkcja odświeżania jest przydatne podczas dodawania nowego rekordu przy użyciu aktualizacji wsadowych wzorca ponieważ automatycznie aktualizuje nowo dodanego `ProductRow` wystąpień `ProductID` właściwości wartościami automatycznie zwiększana przypisane przez bazę danych.

Poniższy kod ilustruje tę funkcję. Zawiera on `ProductsTableAdapter` i `ProductsDataTable` utworzone dla `NorthwindWithSprocs` wpisane zestawu danych. Nowy produkt jest dodawany do bazy danych przez tworzenie `ProductsRow` wystąpienia, podając jego wartości i wywoływania TableAdapter s `Update` metody, przekazując `ProductsDataTable`. Wewnętrznie s TableAdapter `Update` metoda wylicza `ProductsRow` wystąpień w elemencie DataTable przekazany w (w tym przykładzie jest tylko jeden — wystarczy dodać jeden) i wykonuje odpowiednie wstawiania, aktualizowania lub usuwania polecenia. W takim przypadku `Products_Insert` wykonać procedury składowanej, która dodaje nowy rekord do `Products` tabeli i zwraca szczegółów rekordu nowo dodany. `ProductsRow` Wystąpienia s `ProductID` wartość jest następnie aktualizowany. Po `Update` metody została ukończona, firma Microsoft mogą uzyskiwać dostęp do rekordu nowo dodanych s `ProductID` wartości za pośrednictwem `ProductsRow` s `ProductID` właściwości.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

`Products_Update` Podobnie obejmuje procedury składowanej `SELECT` instrukcji po jego `UPDATE` instrukcji.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Należy pamiętać, że procedury składowanej zawiera dwa parametry wejściowe `ProductID`: `@Original_ProductID` i `@ProductID`. Ta funkcja umożliwia w scenariuszach, w którym klucz podstawowy może ulec zmianie. Na przykład w bazie danych pracownika każdego rekordu pracownika może użyć numer ubezpieczenia społecznego s pracownika jako klucz podstawowy. Aby zmienić istniejący numer ubezpieczenia społecznego s pracowników, należy podać numer ubezpieczenia społecznego jak oryginalny. Dla `Products` tabeli tych funkcji nie jest potrzebna, ponieważ `ProductID` kolumna jest `IDENTITY` kolumny i nie można go zmienić. W rzeczywistości `UPDATE` instrukcji w `Products_Update` zawierają procedury składowanej t `ProductID` kolumny na liście kolumn. Tak, podczas gdy `@Original_ProductID` jest używany w `UPDATE` instrukcja s `WHERE` klauzuli jest zbędny dla `Products` tabeli i może zostać zastąpiona `@ProductID` parametru. Podczas modyfikowania parametrów s procedury składowanej ważne jest również aktualizację metody TableAdapter, które używają tej procedury składowanej.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Krok 4: Modyfikowanie parametrów s procedury składowanej i aktualizowanie TableAdapter

Ponieważ `@Original_ProductID` parametr jest zbędny, umożliwiają s usunąć go z `Products_Update` procedury składowanej całkowicie. Otwórz `Products_Update` przechowywane procedury, Usuń `@Original_ProductID` parametru i w `WHERE` klauzuli `UPDATE` instrukcji, Zmień nazwę parametru używane z `@Original_ProductID` do `@ProductID`. Po wprowadzeniu tych zmian, T-SQL w procedurze składowanej powinien wyglądać następująco:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Aby zapisać te zmiany do bazy danych, kliknij ikonę Zapisz na pasku narzędzi, lub kliknij przycisk Ctrl + S. W tym momencie `Products_Update` procedury składowanej nie oczekuje `@Original_ProductID` parametru wejściowego, ale TableAdapter jest skonfigurowany do przekazania taki parametr. Widać parametry TableAdapter będzie wysyłać do `Products_Update` procedury składowanej, wybranie TableAdapter w Projektancie obiektów DataSet, przechodząc do okna właściwości i klikając wielokropek w `UpdateCommand` s `Parameters` kolekcji. Spowoduje to wyświetlenie pokazano na rysunku 14 okno dialogowe Edytor kolekcji parametrów.


![Listy Edytor kolekcji parametrów używanych parametrów przekazanych do Products_Update procedury składowanej](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Rysunek 14**: list Edytor kolekcji parametrów używanych parametrów przekazanych do `Products_Update` procedury składowanej


W tym miejscu można usunąć tego parametru, wybierając po prostu `@Original_ProductID` parametr z listy elementów członkowskich i klikając przycisk Usuń.

Alternatywnie można odświeżyć parametry używane dla wszystkich metod TableAdapter w Projektancie prawym przyciskiem myszy i wybierając pozycję Konfiguruj. Pojawi się Kreator konfiguracji TableAdapter, przechowywanych procedur zaznaczenie, wstawianie, aktualizowanie, wyświetlanie i usuwanie wraz z parametrów procedur składowanych oczekuje się. Jeśli możesz kliknąć pozycję na liście rozwijanej aktualizacji spowoduje `Products_Update` procedur składowanych oczekiwano parametrów wejściowych, które już zawiera teraz `@Original_ProductID` (patrz rysunek 15). Po prostu kliknij przycisk Zakończ, aby automatycznie zaktualizować Kolekcja parametrów używana przez TableAdapter.


[![TableAdapter Kreator konfiguracji s również służy do odświeżania jego kolekcje parametru metody](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Rysunek 15**: Możesz także użyć s TableAdapter Kreator konfiguracji do kolekcji parametrów metod jej odświeżania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Krok 5: Dodawanie metod TableAdapter dodatkowe

Krok 2. pokazano tworząc nowy obiekt TableAdapter jest łatwe mają odpowiednie procedury składowane są generowane automatycznie. Dotyczy to w przypadku dodawania dodatkowych metod do TableAdapter. Na przykład zezwolić Dodaj s `GetProductByProductID(productID)` metodę `ProductsTableAdapter` utworzony w kroku 2. Ta metoda potrwa jako dane wejściowe `ProductID` wartości i zwraca szczegółowe informacje dotyczące określonego produktu.

Uruchom prawym przyciskiem myszy na obiekt TableAdapter i wybierając pozycję Dodaj zapytanie z menu kontekstowego.


![Dodaj nowe zapytanie do TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Rysunek 16**: Dodaj nowe zapytanie do TableAdapter


Spowoduje to uruchomienie Kreatora konfiguracji zapytania TableAdapter, w którym najpierw wyświetla monit dotyczący sposobu TableAdapter powinien uzyskiwać dostęp do bazy danych. Aby utworzyć nową procedurę składowaną, wybierz opcję Utwórz nową opcję procedury składowanej, a następnie kliknij przycisk Dalej.


[![Wybierz nową procedurę składowaną opcję tworzenia](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Rysunek 17**: wybierz opcję Utwórz nową procedurę składowaną opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


Następnym ekranie zapyta firmie Microsoft w celu określenia typu zapytania do wykonania, czy będzie zwraca zestawu wierszy lub pojedynczą wartość skalarną lub wykonać `UPDATE`, `INSERT`, lub `DELETE` instrukcji. Ponieważ `GetProductByProductID(productID)` — metoda będzie zwracać wiersza, pozostaw SELECT, która zwraca wiersz opcja wybrana i kliknij przycisk Dalej.


[![Wybierz SELECT, która zwraca wiersz opcji](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Rysunek 18**: Wybierz SELECT, która zwraca wiersz opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


Na następnym ekranie są wyświetlane s główne zapytanie TableAdapter, która po prostu Wyświetla nazwę procedury składowanej (`dbo.Products_Select`). Zamień na nazwę procedury składowanej następujące `SELECT` instrukcja, która zwraca wszystkie pola produktu dla określonego produktu:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Zamień na nazwę procedury składowanej zapytania SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Rysunek 19**: Zastąp nazwa procedury składowanej z `SELECT` kwerendy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


Kolejne ekranu prośba o nazwę procedury składowanej, która zostanie utworzona. Wprowadź nazwę `Products_SelectByProductID` i kliknij przycisk Dalej.


[![Nazwa nowej Products_SelectByProductID procedury składowanej](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Rysunek 20**: Nazwa nowej procedury składowanej `Products_SelectByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


Ostatni krok kreatora pozwala zmienić metodę nazwy generowane, a także wskazuje, czy należy użyć wypełnienia wzorzec DataTable, zwraca wzorzec DataTable lub oba. Tę metodę, pozostaw obie opcje zaznaczone, ale Zmień nazwę metody `FillByProductID` i `GetProductByProductID`. Kliknij przycisk Dalej, aby wyświetlić podsumowanie kroków kreator przeprowadzi i kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Zmień nazwę metody TableAdapter s FillByProductID i GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Rysunek 21**: Zmień nazwę metody s TableAdapter `FillByProductID` i `GetProductByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


Po zakończeniu działania kreatora TableAdapter ma nową metodę dostępne, `GetProductByProductID(productID)` , gdy została wywołana, będą wykonywane `Products_SelectByProductID` przechowywane procedury, która została właśnie utworzona. Poświęć chwilę, aby wyświetlić ten nowy procedurę składowaną z Eksploratora serwera przechodzenia na wyższy poziom do folderu procedur składowanych i otwierając `Products_SelectByProductID` (Jeśli nie widzisz, kliknij prawym przyciskiem myszy folder na procedury składowane i kliknij przycisk Odśwież).

Należy pamiętać, że `SelectByProductID` przechowywane procedury przyjmuje `@ProductID` jako parametr wejściowy i wykonuje `SELECT` instrukcji, które możemy wprowadzić w kreatorze.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Krok 6: Tworzenie klasy warstwy logiki biznesowej

W samouczku serii firma Microsoft ma strived utrzymania warstwowa architektura, w której warstwie prezentacji wprowadzone wszystkich jego połączeń do firm logiki warstwy (logiki warstwy Biznesowej). Aby stosować się do tej decyzji projektowej, najpierw należy utworzyć klasę logiki warstwy Biznesowej dla nowego typu zestawu danych, aby firma Microsoft dostęp do danych produktu z warstwy prezentacji.

Utwórz nowy plik klasy o nazwie `ProductsBLLWithSprocs.vb` w `~/App_Code/BLL` folderu i Dodaj do niej następujący kod:


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Ta klasa naśladuje `ProductsBLL` klasy semantyki z wcześniejszych samouczków, ale `ProductsTableAdapter` i `ProductsDataTable` obiektów z `NorthwindWithSprocs` zestawu danych. Na przykład zamiast `Imports NorthwindTableAdapters` instrukcji na początku pliku klasy jako `ProductsBLL` tak, `ProductsBLLWithSprocs` klasy używa `Imports NorthwindWithSprocsTableAdapters`. Podobnie `ProductsDataTable` i `ProductsRow` obiektów używanych w tej klasie są poprzedzane prefiksem `NorthwindWithSprocs` przestrzeni nazw. `ProductsBLLWithSprocs` Klasa udostępnia dwie metody, dostępu `GetProducts` i `GetProductByProductID`, oraz metody dodawania, aktualizowania i usuwania wystąpienia jeden produkt.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Krok 7: Praca z`NorthwindWithSprocs`zestawu danych z warstwy prezentacji

W tym momencie utworzono DAL, który używa procedury składowane w celu modyfikacji danych bazy danych i dostępu. Stworzyliśmy również podstawowe logiki warstwy Biznesowej z metod, aby pobrać wszystkie produkty lub określonego produktu oraz metody do dodawania, aktualizowania i usuwania produktów. Do zaokrąglania w tym samouczku, s umożliwiają tworzenie strony platformy ASP.NET, która używa s logiki warstwy Biznesowej `ProductsBLLWithSprocs` klasy wyświetlanie, aktualizowanie i usuwanie rekordów.

Otwórz `NewSprocs.aspx` strony `AdvancedDAL` folder i przeciągnij element GridView z przybornika do projektanta, nadając mu nazwę `Products`. Z widoku GridView tagów inteligentnych s Wybierz powiązać nowy element ObjectDataSource o nazwie `ProductsDataSource`. Element ObjectDataSource umożliwia konfigurowanie `ProductsBLLWithSprocs` klasy, jak pokazano na rysunku 22.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Rysunek 22**: Konfigurowanie ObjectDataSource użyć `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


Listy rozwijanej wybierz karcie ma dwie opcje `GetProducts` i `GetProductByProductID`. Ponieważ chcemy wyświetlić wszystkie produkty w widoku GridView, wybierz `GetProducts` metody. Listy rozwijane w kartach każdego UPDATE, INSERT i DELETE mieć tylko jedną metodę. Sprawdź, czy każdy z tych list rozwijanych ma jego odpowiedniej metody zaznaczone, a następnie kliknij przycisk Zakończ.

Po ukończeniu kreatora ObjectDataSource Visual Studio spowoduje dodanie do widoku GridView dla pól danych produktu BoundFields i CheckBoxField. Włącz GridView s wbudowanych edytowanie i usuwanie funkcji sprawdzając Włącz edytowanie i usuwanie Włącz opcje w tagu.


[![Strona zawiera element GridView z właściwością edytowanie i usuwanie włączona obsługa](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Rysunek 23**: strona zawiera element GridView z właściwością edytowanie i usuwanie włączona obsługa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Jak możemy kolejnych omówione w poprzednich samouczki, po zakończeniu działania kreatora s ObjectDataSource zestawów programu Visual Studio `OldValuesParameterFormatString` właściwości z oryginalną\_{0}. Musi on zostać można przywrócić wartość domyślną {0}, aby funkcje modyfikacji danych działają prawidłowo podane parametry oczekiwany przez metody w naszym logiki warstwy Biznesowej. W związku z tym należy ustawić `OldValuesParameterFormatString` właściwości do {0} lub całkowicie usunąć właściwość z składni deklaratywnej.

Po zakończeniu pracy Kreatora konfigurowania źródła danych, włączenie edycję i usuwanie pomocy technicznej w widoku GridView i zwracanie ObjectDataSource s `OldValuesParameterFormatString` właściwości na wartość domyślną, Twoje s deklaratywne znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

Na tym etapie firma Microsoft można uporządkować widoku GridView dostosowując edycji interfejs do sprawdzania poprawności, obejmują o `CategoryID` i `SupplierID` kolumn renderowane jako DropDownLists i tak dalej. Firma Microsoft może również dodać potwierdzenia po stronie klienta do przycisk Usuń, a I zachęca do czasu implementacji tych rozszerzeń. Ponieważ te tematy zostać omówione w poprzednich samouczki, jednak nie omówimy je ponownie w tym miejscu.

Niezależnie od tego, czy możesz podwyższyć poziom widoku GridView lub nie należy przetestować strony s podstawowe funkcje w przeglądarce. Jak pokazano na rysunku 24, strona zawiera listę produktów w widoku GridView, zapewniająca na wierszu edytowanie i usuwanie funkcji.


[![Produkty można wyświetlać, edytować i usuwane z widoku GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Rysunek 24**: produkty można wyświetlić, edytowany i usuniętych z widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Podsumowanie

TableAdapters w zestawie danych wpisane mają dostęp do danych z bazy danych za pomocą instrukcji SQL ad hoc lub za pomocą procedur składowanych. Podczas pracy z procedur składowanych, albo z istniejącymi procedurami składowanymi mogą być używane lub zalecił TableAdapter Kreator tworzenia nowych przechowywane procedury na podstawie `SELECT` zapytania. W tym samouczku będziemy przedstawione sposobu ustawiania procedur składowanych tworzone automatycznie w firmie Microsoft.

Mając procedur składowanych, który automatycznie generowanej pozwala oszczędzić czas, brak niektórych przypadkach, gdy procedury składowanej utworzone przez kreatora t są wyrównane z co czy został utworzony na własne. Przykładem jest `Products_Update` przechowywane procedury, która oczekiwano zarówno `@Original_ProductID` i `@ProductID` parametrów wejściowych, nawet jeśli `@Original_ProductID` został zbędny parametr.

W wielu scenariuszach procedur składowanych mogło już zostać utworzone lub chcemy je ręcznie, aby mieć dokładniejszą kontrolę nad poleceń procedury przechowywanej s kompilacji. W obu przypadkach czy chcemy poinstruować do użycia z istniejącymi procedurami składowanymi dla jego metody TableAdapter. Są przedstawiono, jak to zrobić w następnym samouczku.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Tworzenie i obsługę procedury składowane](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Pobieranie danych skalarnych z procedury składowanej](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Podstawowe informacje dotyczące procedury przechowywane programu SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedury składowane: Omówienie](http://www.sqlteam.com/item.asp?ItemID=563)
- [Pisanie procedury składowanej](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Hilton Geisenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [dalej](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
