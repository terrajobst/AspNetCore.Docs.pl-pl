---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Tworzenie nowych procedur składowanych dla elementów TableAdapter Typizowanego zestawu danych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W samouczkach wcześniej możemy utworzeniu instrukcji języka SQL w naszym kodzie i przekazywane instrukcje w bazie danych do wykonania. Alternatywnym podejściem jest używać s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 0932749d6cf1665eedd5f452ab5dd63ed8678962
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754610"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Tworzenie nowych procedur składowanych dla elementów TableAdapter Typizowanego zestawu danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) lub [Pobierz plik PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> W samouczkach wcześniej możemy utworzeniu instrukcji języka SQL w naszym kodzie i przekazywane instrukcje w bazie danych do wykonania. Alternatywnym podejściem jest używanie procedur składowanych, w których instrukcje SQL są wstępnie zdefiniowane w bazie danych. W tym samouczku będziemy Dowiedz się, jak Kreator TableAdapter wygenerować nowe procedury składowane dla nas.


## <a name="introduction"></a>Wprowadzenie

Te samouczki warstwy dostępu do danych (DAL) używa wpisanych zestawów danych. Zgodnie z opisem w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) samouczek, zestawów danych w określonym typie składa się z silnie typizowane DataTable i adapterów TableAdapter. DataTable reprezentują jednostki logiczne w systemie podczas interfejsu adapterów TableAdapter z podstawowej bazy danych do wykonywania pracy dostępu do danych. W tym wypełnianie DataTable przy użyciu danych, wykonywania zapytań, które zwracają danych skalarnych, wstawiania, aktualizowania i usuwania rekordów z bazy danych.

Polecenia SQL, wykonywane przez elementów TableAdapter może być albo instrukcji SQL zapytań ad-hoc, takich jak `SELECT columnList FROM TableName`, lub procedur składowanych. TableAdapters w naszej architektury użyj instrukcji SQL zapytań ad-hoc. Wielu programistów i administratorów baz danych, jednak preferowanie procedur składowanych, za pośrednictwem instrukcji SQL zapytań ad-hoc ze względów bezpieczeństwa, łatwość konserwacji i updateability. Preferuj innych ardently instrukcji SQL zapytań ad-hoc dla ich elastyczność. Własnego pracy I Preferuj procedur składowanych w instrukcji SQL zapytań ad-hoc, ale wybrany uprościć samouczki wcześniej za pomocą instrukcji SQL zapytań ad-hoc.

Podczas definiowania TableAdapter lub dodawania nowych metod, Kreator s TableAdapter sprawia, że równie łatwo można utworzyć nowe procedury składowane, lub użyć istniejących procedur składowanych, jak używać instrukcji SQL zapytań ad-hoc. W tym samouczku zajmiemy się, jak Kreator s TableAdapter automatycznego generowania procedur składowanych. W następnym samouczku przedstawiony zostanie sposób konfigurowania metody TableAdapter s, aby użyć istniejącej lub ręcznie utworzone procedur składowanych.

> [!NOTE]
> Zobacz wpis w blogu Rob Howard [nie jeszcze procedur przechowywanych użyj?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) i [Frans Bouma](https://weblogs.asp.net/fbouma/) wpis w blogu s [procedur składowanych są nieprawidłowe, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) dla ich ożywienia debatę na zalet i wad procedury składowane i SQL zapytań ad-hoc.


## <a name="stored-procedure-basics"></a>Podstawowe informacje dotyczące procedury składowanej

Funkcje są konstrukcji, które są wspólne dla wszystkich języków programowania. Funkcja jest zbiór instrukcji, które są wykonywane, gdy wywoływana jest funkcja. Funkcje może akceptować parametry wejściowe i opcjonalnie może zwracać wartości. *[Procedury składowane](http://en.wikipedia.org/wiki/Stored_procedure)*  są konstrukcje bazy danych, które mają wiele wspólnego z usługą functions w językach programowania. Procedura składowana składa się z zestawu instrukcji języka T-SQL, które są wykonywane, gdy zostanie wywołana procedura składowana. Procedura składowana może akceptować zero do wielu parametrów wejściowych i może zwrócić wartości skalarnych, parametry wyjściowe lub najczęściej zestawy wyników z `SELECT` zapytania.

> [!NOTE]
> Procedury składowane są często określane jako sprocs lub dodatki Service Pack.


Procedury składowane są tworzone przy użyciu [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) instrukcję języka T-SQL. Na przykład, poniższy skrypt języka T-SQL tworzy procedury składowanej o nazwie `GetProductsByCategoryID` , przyjmuje jeden parametr o nazwie `@CategoryID` i zwraca `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` pola te kolumny w `Products` tabeli, który ma odpowiadający mu `CategoryID` wartość:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Po utworzeniu tej procedury składowanej może być wywoływana przy użyciu następującej składni:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> W następnym samouczku będziemy sprawdzać Tworzenie procedur składowanych za pośrednictwem środowiska IDE programu Visual Studio. W tym samouczku jednak zamierzamy pozwolić kreatorowi TableAdapter automatycznego generowania procedur składowanych dla nas.


Oprócz po prostu zwraca dane, procedury składowane są często używane do wykonywania wielu poleceń bazy danych w zakresie pojedynczą transakcję. Procedura składowana o nazwie `DeleteCategory`, na przykład może zająć `@CategoryID` parametru i wykonywania dwóch `DELETE` instrukcji:, pierwsza do usuwania powiązanych z nimi produktów i drugi, usunięcie określonej kategorii. Użycie wielu instrukcji w procedurze składowanej są *nie* automatycznie zawijany w obrębie transakcji. Dodatkowe polecenia języka T-SQL muszą wydawane zapewnienie procedury składowanej s, w których wielu poleceń są traktowane jako operację niepodzielną. Zobaczymy, jak opakowywać polecenia s procedurę składowaną w ramach zakresu transakcji w kolejnym samouczku.

Korzystając z procedur składowanych w ramach architektury, metody s warstwy dostępu do danych wywołania określonej procedury składowanej niż wystawianie instrukcji SQL zapytań ad-hoc. Lokalizacja instrukcji SQL, wykonywane (w bazie danych) umożliwia scentralizowanie zamiast on zdefiniowany w ramach architektury s aplikacji. Taka Centralizacja prawdopodobnie sprawia, że łatwiej znaleźć, analizowania i dostrojenia zapytań i udostępnia wiele lepszy obraz tego, gdzie i jak baza danych jest używana.

Aby uzyskać więcej informacji na temat procedury składowanej podstawy zapoznaj się zasoby przedstawione w sekcji dalsze informacje na końcu tego samouczka.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Krok 1: Tworzenie stron sieci Web scenariuszy warstwy dostępu do zaawansowanych danych

Zanim zaczniemy naszych dyskusji na temat tworzenia DAL, korzystanie z procedur składowanych umożliwiają s najpierw Poświęć chwilę, aby utworzyć strony ASP.NET w projekcie naszej witryny sieci Web, które są wymagane dla tego programu oraz dalej kilka samouczków. Rozpocznij od dodania nowy folder o nazwie `AdvancedDAL`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Dodawanie stron ASP.NET samouczki scenariuszy warstwy dostępu do zaawansowanych danych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Rysunek 1**: Dodawanie stron ASP.NET samouczki scenariuszy warstwy dostępu do zaawansowanych danych


Podobnie jak w przypadku innych folderów `Default.aspx` w `AdvancedDAL` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Rysunek 2**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Wreszcie, Dodaj te strony jako wpisy, aby `Web.sitemap` pliku. W szczególności należy dodać następujące znaczniki po zakończeniu pracy z danymi w partiach `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementów dla samouczki zaawansowane scenariusze warstwy DAL.


![Mapa witryny zawiera teraz wpisy samouczki scenariuszy zaawansowanych warstwy DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Rysunek 3**: mapy witryny zawiera teraz wpisy samouczki scenariuszy zaawansowanych warstwy DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Krok 2: Konfigurowanie TableAdapter w celu utworzenia nowych procedur składowanych

Aby zademonstrować, Tworzenie warstwy dostępu do danych, która używa procedur składowanych zamiast instrukcji SQL zapytań ad-hoc, umożliwiają s tworzenie nowych wpisana zestawu danych w `~/App_Code/DAL` folder o nazwie `NorthwindWithSprocs.xsd`. Ponieważ firma Microsoft ma zmieniana przez ten proces szczegółowo w poprzednich samouczkach, firma Microsoft przejdą szybko kroków w tym miejscu. Jeśli masz problem lub konieczne dalsze instrukcje krok po kroku w tworzenie i konfigurowanie zestawu danych wpisane odnoszą się do [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-cs.md) samouczka.

Dodaj nowy zestaw danych do projektu, klikając prawym przyciskiem myszy `DAL` folderu, wybierając Dodaj nowy element, a następnie wybierając szablon zestawu danych, jak pokazano na rysunku 4.


[![Dodaj nowy Typizowany zestaw danych do projektu o nazwie NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Rysunek 4**: Dodaj nowy zestaw danych wpisany do projektu o nazwie `NorthwindWithSprocs.xsd` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Spowoduje to utworzenie nowego zestawu danych wpisane, otwórz jego projektanta, Utwórz nowy obiekt TableAdapter i uruchom Kreator konfiguracji TableAdapter. Pierwszym krokiem s Kreator konfiguracji TableAdapter pyta, czy nam wybrać bazę danych do pracy z. Parametry połączenia z bazą danych Northwind powinny wymienione na liście rozwijanej. Wybierz tę opcję, a następnie kliknij przycisk Dalej.

Na tym ekranie dalej będziemy wybierz, jak TableAdapter powinien uzyskiwać dostęp do bazy danych. W poprzednich samouczkach Wybraliśmy pierwszej opcji, użyj instrukcji SQL. Na potrzeby tego samouczka wybierz drugą opcję Utwórz nowe procedury składowane i kliknij przycisk Dalej.


[![Poinstruuj TableAdpater, aby utworzyć nowe procedury składowane](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Rysunek 5**: poinstruować TableAdpater na tworzenie nowych procedur składowanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Tak samo, jak przy użyciu instrukcji SQL zapytań ad-hoc, w następnym kroku będziemy prośba o podanie `SELECT` instrukcji kwerendy głównych s TableAdapter. Jednak zamiast klauzuli `SELECT` instrukcji wprowadzone w tym miejscu można wykonać zapytania ad hoc bezpośrednio, Kreator s TableAdapter ma utworzyć procedurę składowaną, która zawiera to `SELECT` zapytania.

Należy użyć następującego `SELECT` zapytania dla tego TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Wprowadź zapytanie SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Rysunek 6**: wprowadź `SELECT` zapytania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> Zapytanie różni się nieco od głównego zapytania `ProductsTableAdapter` w `Northwind` wpisana zestawu danych. Pamiętamy `ProductsTableAdapter` w `Northwind` wpisany zestaw danych zawiera dwa podzapytań skorelowany przywrócić nazwa kategorii i nazwę firmy, dla każdej kategorii produktów s i dostawcy. W przyszłych [aktualizowanie elementu TableAdapter w celu użyj łączy](updating-the-tableadapter-to-use-joins-cs.md) danych związanych z samouczka przyjrzymy się dodając następujący kod do tego TableAdapter.


Poświęć chwilę, kliknij przycisk Opcje zaawansowane. W tym miejscu możemy określić, czy kreator powinien również wygenerować insert, update i usuwania instrukcji TableAdapter, czy użyć optymistycznej współbieżności i tego, czy tabela danych powinny być odświeżane po operacji wstawienia i aktualizacje. Generowanie Insert, Update i Delete instrukcji opcja jest zaznaczona domyślnie. Pozostaw je zaznaczone. W tym samouczku zaznaczaj opcji Użyj optymistycznej współbieżności.

Mając przechowywanych procedur, które są tworzone automatycznie przez kreatora TableAdapter wydaje się, że odświeżanie opcji tabeli danych jest ignorowana. Niezależnie od tego, czy to pole wyboru jest zaznaczone pole wyboru, wynikowy insert i update procedur składowanych pobrać rekordu just wstawione lub zaktualizowane po prostu, zobaczymy w kroku 3.


![Pozostaw instrukcje generowania Insert, Update i Delete, zaznaczone pole](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Rysunek 7**: pozostaw instrukcje generowania Insert, Update i Delete, zaznaczone pole


> [!NOTE]
> Jeśli zaznaczono opcję Użyj optymistycznej współbieżności, Kreator doda dodatkowe warunki `WHERE` klauzula, która uniemożliwiają aktualizowana, jeśli wprowadzono zmiany w pozostałych polach danych. Odwołaj się do [Implementowanie optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) samouczka, aby uzyskać więcej informacji na temat korzystania z funkcji sterowania TableAdapter s wbudowanych optymistycznej współbieżności.


Po wprowadzeniu `SELECT` zapytania i potwierdzenie, że zaznaczono opcję instrukcje generowania Insert, Update i Delete, kliknij przycisk Dalej. Ta następnym ekranie pokazano na rysunku 8 monituje o podanie nazwy procedur składowanych, który Kreator ma utworzyć wybierając, wstawianie, aktualizowanie i usuwanie danych. Zmiany są przechowywane nazwy procedury `Products_Select`, `Products_Insert`, `Products_Update`, i `Products_Delete`.


[![Zmień nazwę procedury składowane](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Rysunek 8**: zmiana nazwy procedur składowanych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Aby wyświetlić kreatora TableAdapter zostaną użyte do utworzenia czterech procedur składowanych języka T-SQL, kliknij przycisk Podgląd skryptu SQL. W oknie dialogowym skryptu SQL (wersja zapoznawcza) możesz zapisać skrypt do pliku lub skopiuj go do Schowka.


![Podgląd skryptu SQL używanego do wygenerowania procedur składowanych](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Rysunek 9**: Podgląd skryptu SQL używanego do generowania procedur składowanych


Po nazw procedur składowanych, kliknij obok nazwy TableAdapter s odpowiadających im metod. Podobnie jak podczas przy użyciu instrukcji SQL zapytań ad-hoc możemy utworzyć metody fill istniejącego elementu DataTable lub zwracać nową. Firma Microsoft można również określić, czy TableAdapter powinien zawierać wzorca bazy danych bezpośrednio do wstawiania, aktualizowania i usuwania rekordów. Pozostaw zaznaczone wszystkie trzy pola wyboru, ale Zmień nazwę zwrotu metody DataTable `GetProducts` (jak pokazano na rysunku nr 10).


[![Nazwa metody GetProducts i wypełnienia](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Na rysunku nr 10**: Nazwa metody `Fill` i `GetProducts` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Kliknij przycisk Dalej, aby wyświetlić podsumowanie działań, które wykona Kreator. Ukończ pracę kreatora, klikając przycisk Zakończ. Po zakończeniu pracy Kreatora nastąpi powrót do s DataSet Designer, która teraz powinna zawierać `ProductsDataTable`.


[![S Projektanta obiektów DataSet zawiera nowo dodanych ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Rysunek 11**: DataSet s Designer przedstawia nowo dodane `ProductsDataTable` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Krok 3: Sprawdzenie nowo utworzone procedury składowane

TableAdapter Kreator automatycznie użyte w kroku 2 stworzone procedury składowane dla wybierając, wstawianie, aktualizowanie i usuwanie danych. Te procedury składowane można przeglądać lub modyfikować za pomocą programu Visual Studio, przechodząc do Eksploratora serwera i przechodzenie do szczegółów w folderze procedur składowanych s bazy danych. Jak pokazano na rysunku 12, bazy danych Northwind zawiera cztery nowe procedury składowane: `Products_Delete`, `Products_Insert`, `Products_Select`, i `Products_Update`.


![Cztery procedury przechowywane utworzone w kroku 2 można znaleźć w folderze procedur składowanych bazy danych s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Rysunek 12**: czterech procedur składowanych, utworzony w kroku 2 można znaleźć w folderze procedur składowanych bazy danych s


> [!NOTE]
> Jeśli nie widzisz Eksploratora serwera, przejdź do menu widoku i wybierz opcję Eksploratora serwera. Jeśli nie widzisz związane z produktem procedur składowanych dodane z kroku nr 2, spróbuj, klikając prawym przyciskiem myszy w folderze procedur składowanych i wybierając polecenie Odśwież.


Aby wyświetlić lub zmodyfikować procedury składowanej, kliknij dwukrotnie jego nazwę w Eksploratorze serwera lub Alternatywnie kliknij prawym przyciskiem myszy na procedury składowanej i wybierz przycisk Otwórz. Przedstawia rysunek 13 `Products_Delete` procedurą składowaną, po otwarciu.


[![Procedury składowane można go otworzyć i zmodyfikować z poziomu programu Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Rysunek 13**: przechowywane procedury można otwierać i modyfikować z w ramach programu Visual Studio ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


Zawartość obu `Products_Delete` i `Products_Select` procedury składowane są bardzo proste. `Products_Insert` i `Products_Update` procedur składowanych z drugiej strony gwarantuje ściślejszej kontroli, wykonujących zarówno `SELECT` instrukcji znajdującej się po ich `INSERT` i `UPDATE` instrukcji. Na przykład, następujące instrukcje SQL stanowi `Products_Insert` procedura składowana:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Procedura składowana przyjmuje jako parametry wejściowe `Products` kolumn, które zostały zwrócone przez `SELECT` zapytanie określone w Kreatorze s TableAdapter i te wartości są używane w `INSERT` instrukcji. Następujące `INSERT` instrukcji `SELECT` zapytanie służy do zwracania `Products` wartości w kolumnie (w tym `ProductID`) nowo dodanego rekordu. Ta funkcja odświeżania jest przydatne podczas dodawania nowego rekordu przy użyciu wzorca aktualizacji usługi Batch jako go automatycznie aktualizuje nowo dodanych `ProductRow` wystąpień `ProductID` właściwości wartościami automatycznie zwiększana przypisany przez bazę danych.

Poniższy kod ilustruje tę funkcję. Zawiera on `ProductsTableAdapter` i `ProductsDataTable` utworzone dla `NorthwindWithSprocs` wpisana zestawu danych. Nowy produkt jest dodawany do bazy danych, tworząc `ProductsRow` wystąpienia, podając jego wartości i wywoływania TableAdapter s `Update` metody, przekazując `ProductsDataTable`. Wewnętrznie s TableAdapter `Update` metoda wylicza `ProductsRow` wystąpień przekazywane w tabeli DataTable (w tym przykładzie jest tylko jeden — jeden właśnie dodaliśmy) i wykonuje odpowiednie wstawiania, aktualizacji lub usuwania polecenia. W tym przypadku `Products_Insert` wykonywane procedury składowanej, która dodaje nowy rekord do `Products` tabeli i zwraca szczegółowe informacje o nowo dodany rekord. `ProductsRow` Wystąpienia s `ProductID` wartość jest następnie aktualizowany. Po `Update` metoda została ukończona, firma Microsoft mogą uzyskiwać dostęp do nowo dodany rekord s `ProductID` wartości za pomocą `ProductsRow` s `ProductID` właściwości.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update` Podobnie obejmuje procedury składowanej `SELECT` instrukcji znajdującej się po jego `UPDATE` instrukcji.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Należy pamiętać, że procedura składowana zawiera dwa parametry wejściowe dla `ProductID`: `@Original_ProductID` i `@ProductID`. Ta funkcjonalność umożliwia dla scenariuszy, w którym klucz podstawowy może ulec zmianie. Na przykład w bazie danych pracownika każdego wybranego rekordu pracownika może używać numer ubezpieczenia społecznego s pracowników jako ich klucz podstawowy. Aby zmienić istniejący numer ubezpieczenia społecznego s pracowników, należy podać nowy numer ubezpieczenia społecznego i oryginalnego. Dla `Products` tabeli, a funkcjonalność nie jest potrzebna, ponieważ `ProductID` kolumna jest `IDENTITY` kolumny i nigdy nie powinna być zmieniana. W rzeczywistości `UPDATE` instrukcji w `Products_Update` obejmują procedury składowanej t `ProductID` kolumny na liście kolumn. Tak, podczas gdy `@Original_ProductID` jest używany w `UPDATE` instrukcja s `WHERE` klauzula jest zbędny dla `Products` tabeli i może zostać zastąpiona `@ProductID` parametru. Podczas modyfikowania parametrów s procedury składowanej ważne jest, że metody TableAdapter, które używają tej procedury składowanej również są aktualizowane.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Krok 4: Modyfikowanie parametrów s procedury składowanej i aktualizowanie elementu TableAdapter

Ponieważ `@Original_ProductID` parametrów jest zbędny, umożliwiają s, usuń go z `Products_Update` procedury składowanej całkowicie. Otwórz `Products_Update` procedurą składowaną, Usuń `@Original_ProductID` parametru, a następnie w `WHERE` klauzuli `UPDATE` instrukcji, Zmień nazwę parametru używane z `@Original_ProductID` do `@ProductID`. Po wprowadzeniu tych zmian, T-SQL w procedurze składowanej powinien wyglądać następująco:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Aby zapisać te zmiany do bazy danych, kliknij ikonę Zapisz na pasku narzędzi lub naciśnij klawisze Ctrl + S. W tym momencie `Products_Update` procedury składowanej nie oczekuje `@Original_ProductID` parametr wejściowy, ale TableAdapter jest skonfigurowany do przekazania takich parametrów. Możesz zobaczyć parametry TableAdapter będzie wysyłać do `Products_Update` procedury składowanej, wybranie elementu TableAdapter w Projektancie obiektów DataSet, przechodząc do okna właściwości i klikając wielokropek w `UpdateCommand` s `Parameters` kolekcji. Wywołuje okno dialogowe Edytor kolekcji parametrów pokazano na rysunku 14.


![List — Edytor kolekcji parametrów parametry używane jest przekazywany do Products_Update procedury składowanej](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Rysunek 14**: Wyświetla Edytor kolekcji parametrów parametry używane są przekazywane do `Products_Update` procedury składowanej


W tym miejscu można usunąć tego parametru, po prostu wybierając `@Original_ProductID` parametr z listy elementów członkowskich i klikając przycisk Usuń.

Możesz też odświeżyć parametry używane dla wszystkich metod, klikając prawym przyciskiem myszy na obiekt TableAdapter w Projektancie i wybierając pozycję Konfiguruj. Pojawi się Kreator konfiguracji TableAdapter, listę procedur składowanych, umożliwiający wybieranie, wstawianie, aktualizowanie i usuwanie wraz z parametrami procedur składowanych oczekiwać. Jeśli możesz kliknąć na liście rozwijanej aktualizacji możesz zobaczyć `Products_Update` procedur składowanych oczekiwano parametrów wejściowych, które nie są już zawiera teraz `@Original_ProductID` (zobacz rysunek 15). Po prostu kliknij Zakończ, aby automatycznie aktualizować posługują się TableAdapter kolekcji parametrów.


[![Można też użyć Kreator konfiguracji TableAdapter s odświeżyć jego kolekcjach parametru metody](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Rysunek 15**: Alternatywnie można użyć s TableAdapter Kreator konfiguracji do kolekcji parametrów metody jego odświeżania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Krok 5: Dodawanie metod TableAdapter dodatkowe

Krok 2. pokazano tworząc nowy obiekt TableAdapter jest proste zapewnienie odpowiedniej procedury składowane generowane automatycznie. Dotyczy to także podczas dodawania dodatkowych metod do TableAdapter. Na przykład umożliwiają dodawanie s `GetProductByProductID(productID)` metody `ProductsTableAdapter` utworzony w kroku 2. Ta metoda zajmie jako dane wejściowe `ProductID` wartość i zwrócić szczegółowe informacje dotyczące określonego produktu.

Rozpocznij, klikając prawym przyciskiem myszy na obiekt TableAdapter i wybierając pozycję Dodaj zapytanie, z menu kontekstowego.


![Dodaj nowe zapytanie do elementu TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Rysunek 16**: Dodaj nowe zapytanie do elementu TableAdapter


Spowoduje to uruchomienie Kreatora konfiguracji zapytania TableAdapter najpierw wyświetla monit dotyczący sposobu TableAdapter powinien uzyskiwać dostęp do bazy danych. Aby utworzyć procedurę przechowywaną, opcję tworzenia nowej procedury składowanej, a następnie kliknij przycisk Dalej.


[![Wybierz pozycję Utwórz nową procedurę składowaną opcji](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Rysunek 17**: wybierz pozycję Utwórz nową procedurę składowaną opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


Następny ekran prosi nam identyfikują typ kwerenda do wykonania, czy będzie zwrócić zestaw wierszy lub pojedynczą wartość skalarną lub wykonać `UPDATE`, `INSERT`, lub `DELETE` instrukcji. Ponieważ `GetProductByProductID(productID)` metody będzie zwracać wiersz, pozostaw SELECT, która zwraca, opcja wiersza zaznaczone i kliknij Dalej.


[![Wybierz polecenie SELECT, która zwraca wiersz — opcja](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Rysunek 18**: Wybierz SELECT, która zwraca wiersz opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


Na następnym ekranie są wyświetlane do TableAdapter s głównego zapytania, które po prostu Wyświetla nazwę procedury składowanej (`dbo.Products_Select`). Zamień na nazwę procedury składowanej następujące `SELECT` instrukcję, która zwraca wszystkie pola produktu dla określonego produktu:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Zamień na nazwę procedury składowanej zapytania SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Rysunek 19**: Zastąp nazwa procedury składowanej z `SELECT` zapytania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


Kolejne ekranu prosi o nazwę procedury przechowywanej, która zostanie utworzona. Wprowadź nazwę `Products_SelectByProductID` i kliknij przycisk Dalej.


[![Nazwa nowego Products_SelectByProductID procedury składowanej](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Rysunek 20**: Nazwij nową procedurę przechowywaną `Products_SelectByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


Ostatnim krokiem Kreator pozwala zmienić metodę nazwy wygenerowane, jak również wskazać, czy ma być używany do wypełnienia DataTable wzorzec, wróć do wzorca DataTable i / lub. W przypadku tej metody należy pozostawić obie opcje zaznaczone, ale Zmień nazwę metody służące do `FillByProductID` i `GetProductByProductID`. Kliknij przycisk Dalej, aby wyświetlić podsumowanie kroków kreatora zostanie wykonywania, a następnie kliknij przycisk Zakończ, aby zakończyć pracę kreatora.


[![Zmień nazwę metody s TableAdapter FillByProductID i GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Rysunek 21**: Zmień nazwę metody s TableAdapter `FillByProductID` i `GetProductByProductID` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Po zakończeniu działania kreatora TableAdapter ma nową metodę dostępne `GetProductByProductID(productID)` , wywołana zostanie wykonana `Products_SelectByProductID` przechowywane procedury, która właśnie została utworzona. Poświęć chwilę, aby wyświetlić tej nowej procedury składowanej z poziomu Eksploratora serwera przechodzenia do szczegółów w folderze procedur składowanych i otwierając `Products_SelectByProductID` (Jeśli nie widzisz, kliknij prawym przyciskiem myszy w folderze procedur składowanych i wybierz polecenie Odśwież).

Należy pamiętać, że `SelectByProductID` przechowywane procedury przyjmuje `@ProductID` jako parametr wejściowy i wykonuje `SELECT` instrukcji, które możemy wprowadzić w kreatorze.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Krok 6: Tworzenie klasy warstwy logiki biznesowej

W całej serii samouczków firma Microsoft ma strived Obsługa architektury warstwowej, w których są wykonane wszystkie jego wywołań do warstwy logiki biznesowej (LOGIKI) warstwy prezentacji. Aby stosować się do tej decyzji projektowej, najpierw należy utworzyć klasę LOGIKI dla nowego zestawu danych wpisane, firma Microsoft dostęp do danych produktu z warstwy prezentacji.

Utwórz nowy plik klasy o nazwie `ProductsBLLWithSprocs.cs` w `~/App_Code/BLL` folderze i Dodaj do niej następujący kod:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Ta klasa naśladuje `ProductsBLL` semantyki z wcześniejszych samouczków, ale używa klasy `ProductsTableAdapter` i `ProductsDataTable` obiekty `NorthwindWithSprocs` zestawu danych. Na przykład, zamiast `using NorthwindTableAdapters` instrukcji na początku pliku klasy jako `ProductsBLL` , działa `ProductsBLLWithSprocs` klasy używa `using NorthwindWithSprocsTableAdapters`. Podobnie `ProductsDataTable` i `ProductsRow` mają prefiks obiektów używanych w tej klasie `NorthwindWithSprocs` przestrzeni nazw. `ProductsBLLWithSprocs` Udostępnia dwie dostęp do metod, `GetProducts` i `GetProductByProductID`, oraz metody dodawania, aktualizowania i usuwania wystąpień jednego produktu.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Krok 7: Praca z`NorthwindWithSprocs`zestawu danych z warstwy prezentacji

W tym momencie utworzyliśmy DAL, korzystającą z procedur składowanych w celu dostępu i modyfikowania danych bazowych bazy danych. Nawiązaliśmy również podstawowe LOGIKI za pomocą metod, aby pobrać wszystkie produkty lub konkretnego produktu oraz metody dodawania, aktualizowania i usuwania produktów. Aby zaokrąglić w tym samouczku, umożliwiają s utworzyć strony ASP.NET, która używa s LOGIKI `ProductsBLLWithSprocs` klasę wyświetlanie, aktualizowanie i usuwanie rekordów.

Otwórz `NewSprocs.aspx` strony w `AdvancedDAL` folder i przeciągnij GridView z przybornika w projektancie, nadając mu nazwę `Products`. Z widoku GridView chce powiązać nowe kontrolki ObjectDataSource, o nazwie tagu inteligentnego s `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLLWithSprocs` klasy, jak pokazano na rysunku 22.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Rysunek 22**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsBLLWithSprocs` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


Listy rozwijanej wybierz karcie ma dwie opcje `GetProducts` i `GetProductByProductID`. Ponieważ chcemy wyświetlić wszystkie produkty w widoku GridView, wybrać `GetProducts` metody. Listy rozwijane w kartach poszczególnych aktualizacji, WSTAWIANIA i usuwania mieć tylko jedną z metod. Upewnić się, że każda z tych list rozwijanych jego odpowiedniej metody zaznaczone, a następnie kliknij przycisk Zakończ.

Po zakończeniu pracy Kreatora ObjectDataSource, Visual Studio spowoduje dodanie BoundFields i CheckBoxField GridView dla pól danych produktu. Włącz GridView s wbudowanego edytowania i usuwania funkcji, sprawdzając Włącz edytowanie i Włącz usuwanie opcji w tagu inteligentnego.


[![Ta strona zawiera GridView za pomocą edytowania i usuwania włączona obsługa](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Ilustracja 23**: strona zawiera GridView za pomocą edycji i usuwania włączona obsługa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Ponieważ ve omówione w poprzednich samouczkach po ukończeniu działania kreatora s ObjectDataSource zestawów programu Visual Studio `OldValuesParameterFormatString` właściwości z oryginalną\_{0}. Jest to konieczne, należy najpierw przywrócić jego wartość domyślna {0} w kolejności dla funkcji modyfikacji danych zapewnić prawidłowe działanie podanymi jako parametry oczekiwana przez metody w naszym LOGIKI. W związku z tym, należy ustawić `OldValuesParameterFormatString` właściwości {0} lub całkowicie usunąć właściwość z składni deklaratywnej.

Po zakończeniu pracy kreatora Konfigurowanie źródła danych, włączając edycji i usuwania pomocy technicznej w widoku GridView i powrocie ObjectDataSource s `OldValuesParameterFormatString` właściwości do wartości domyślnej, Twoja oznaczeniu deklaracyjnym strony s powinien wyglądać podobnie do następujących:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

W tym momencie możemy można uporządkować dane widoku GridView przez dostosowywanie interfejsu edycji, aby uwzględnić sprawdzania poprawności, posiadające `CategoryID` i `SupplierID` kolumn renderowane jako kontrolek DROPDOWNLIST i tak dalej. Firma Microsoft może również dodać potwierdzenia po stronie klienta do przycisk Usuń, a zachęcam Cię do Poświęć chwilę Aby zaimplementować te rozszerzenia funkcjonalności. Ponieważ te tematy zostać omówione w poprzednich samouczkach, jednak nie omówimy je ponownie w tym miejscu.

Niezależnie od tego, czy należy podwyższyć poziom widoku GridView lub nie przetestowania strony s podstawowe funkcje w przeglądarce. Jak pokazano na rysunku 24, strona zawiera listę produktów w GridView zapewniająca — w wierszu edytowania i usuwania możliwości.


[![Produkty można wyświetlać, edytować i usuwane z widoku GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Rysunek 24**: produkty mogą być wyświetlane, edytowana i usunięte z kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Podsumowanie

TableAdapter w zestawie danych wpisane mają dostęp do danych z bazy danych przy użyciu zapytań ad-hoc instrukcji SQL lub procedur składowanych. Podczas pracy z procedur składowanych, albo istniejące procedury składowane mogą być używane, lub metoda TableAdapter kreator Utwórz nowy procedury składowane na podstawie `SELECT` zapytania. W tym samouczku rozważyliśmy jak procedur składowanych tworzone automatycznie dla nas.

Mając procedur składowanych, które automatycznie generowanej pozwala oszczędzić czas, istnieją niektóre przypadki, gdzie procedury składowanej utworzone przez kreatora t było zgodne z co będzie utworzyliśmy na własną. Przykładem jest `Products_Update` procedurą składowaną, którego oczekiwano zarówno `@Original_ProductID` i `@ProductID` parametrów wejściowych, nawet jeśli `@Original_ProductID` parametr jest zbędny.

W wielu scenariuszach procedur składowanych mogło już zostać utworzone lub warto tworzyć je ręcznie, aby mieć lepszą kontrolę nad poleceń s procedury składowanej. W obu przypadkach firma Microsoft będzie chciała poinstruować TableAdapter do użycia z istniejącymi procedurami składowanymi dla jego metody. Firma Microsoft jest Zobacz, jak w tym celu w następnym samouczku.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Utworzenie i utrzymywanie procedur składowanych](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Pobieranie danych skalarną z procedury składowanej](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Program SQL Server przechowywane procedury podstawy](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedury składowane: Omówienie](http://www.sqlteam.com/item.asp?ItemID=563)
- [Zapisywanie procedury składowanej](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Hilton Geisenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
