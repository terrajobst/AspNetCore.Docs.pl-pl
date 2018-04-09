---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Konfigurowanie ustawień połączenia i polecenia poziom Warstwa dostępu do danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: TableAdapters w elemencie DataSet wpisany automatycznie zajmie się łączenie z bazą danych, wydawania polecenia i wypełniania elementu DataTable z wynikami...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: b73c6113e84e290025e5835781fa2f85587289b1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Konfigurowanie ustawień połączenia i polecenia poziom Warstwa dostępu do danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) lub [pobierania plików PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> TableAdapters w elemencie DataSet wpisany automatycznie zajmie się łączenie z bazą danych, wydawania polecenia i wypełniania elementu DataTable z wynikami. Jednak gdy chcemy zajmie się nad szczegółowe informacje, a w tym samouczku będziemy sposób uzyskiwania dostępu do ustawienia połączenia i polecenia poziomu bazy danych w TableAdapter jest zmieniana.


## <a name="introduction"></a>Wprowadzenie

W samouczku serii użyliśmy wpisanych zestawów danych do zaimplementowania Warstwa dostępu do danych i obiektów biznesowych o Nasza architektura warstwowego. Zgodnie z opisem w [pierwszy samouczek](../introduction/creating-a-data-access-layer-vb.md), s wpisane DataSet DataTables stanowić repozytoria danych TableAdapters działać jako otoki do komunikowania się z bazą danych, aby pobrać i zmodyfikować danych. TableAdapters Hermetyzowanie złożoności procesu w pracy z bazą danych i zapisuje nam z konieczności pisania kodu w celu połączenia z bazą danych, wydać polecenie lub wypełnić wyniki do elementu DataTable.

Brak sytuacji, gdy musimy burrow do głębokości TableAdapter i pisania kodu, który współpracuje bezpośrednio z obiektów ADO.NET. W [zawijania modyfikacje bazy danych w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) samouczek, na przykład dodaliśmy metod do TableAdapter na początku, zatwierdzania i wycofywanie transakcji ADO.NET. Te metody używane wewnętrznego, utworzone ręcznie `SqlTransaction` obiektu, który został przypisany do TableAdapter s `SqlCommand` obiektów.

W tym samouczku omówione jak uzyskać dostęp do ustawień połączenia i polecenia poziomu bazy danych w metodzie TableAdapter. W szczególności spowoduje dodanie funkcji `ProductsTableAdapter` czy umożliwia dostęp do podstawowych parametrów połączenia i ustawienia limitu czasu polecenia.

## <a name="working-with-data-using-adonet"></a>Praca z danymi za pomocą ADO.NET

Microsoft .NET Framework zawiera nadmiar klasy zaprojektowany specjalnie w celu pracy z danymi. Te klasy w [ `System.Data` przestrzeni nazw](https://msdn.microsoft.com/library/system.data.aspx), są określane jako *ADO.NET* klasy. Niektóre z klas w obszarze parasola ADO.NET są powiązane z określonego *dostawcy danych*. Dostawca danych można traktować jako kanał komunikacyjny, który umożliwia informacji między klasami ADO.NET i magazyn danych. Brak dostawców ogólnych, takich jak OLE DB i ODBC, a także dostawców przeznaczone specjalnie dla określonej bazy danych systemu. Na przykład gdy istnieje możliwość połączenia z bazą danych programu Microsoft SQL Server przy użyciu dostawcy OleDb, dostawca SqlClient jest bardziej efektywnego został zaprojektowany i zoptymalizowany specjalnie dla programu SQL Server.

Gdy programowane uzyskiwanie dostępu do danych, często służy następującego wzorca:

1. Nawiąż połączenie z bazą danych.
2. Wydać polecenie.
3. Aby uzyskać `SELECT` zapytania, pracować z wynikowe rekordy.

Brak osobnych klas ADO.NET do wykonywania każdego z tych kroków. Aby połączyć się z bazą danych przy użyciu dostawcy SqlClient, na przykład użyć [ `SqlConnection` klasy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Do wystawienia `INSERT`, `UPDATE`, `DELETE`, lub `SELECT` do bazy danych, użyj polecenia [ `SqlCommand` klasy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Z wyjątkiem [zawijania modyfikacje bazy danych w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) samouczek, nie mamy pisać ADO.NET niskiego poziomu nad kodu, ponieważ TableAdapters automatycznie wygenerowany kod zawiera funkcje niezbędne do połączenie z bazą danych, wydawać polecenia pobierania danych i wypełnić tych danych do DataTables. Jednak może być razy podczas musimy dostosować te ustawienia niskiego poziomu. W następnych kilku krokach omówione jak dostęp do obiektów ADO.NET używana wewnętrznie przez TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Krok 1: Sprawdzenie za pomocą właściwości połączenia

Każda klasa TableAdapter ma `Connection` właściwość, która określa informacje dotyczące połączenia bazy danych. Ten typ danych właściwości s i `ConnectionString` wartości są określane przez wybrane w Kreatorze konfiguracji TableAdapter. Odwołaj, że gdy TableAdapter możemy najpierw dodać do zestawu danych typu ten kreator zapyta, nam dla bazy danych źródła (zobacz rysunek 1). Listy rozwijanej w pierwszym kroku obejmuje tych baz danych określonych w pliku konfiguracji, a także innych baz danych w Eksploratorze serwera s połączeń danych. Jeśli baza danych, którą chcemy użyć nie istnieje na liście rozwijanej, można określić nowego połączenia z bazą danych przez kliknięcie przycisku nowe połączenie i udostępnia informacje o połączeniu potrzebne.


[![Pierwszym krokiem w Kreatorze konfiguracji TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Rysunek 1**: pierwszy krok w Kreatorze konfiguracji TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Let s Poświęć chwilę, aby sprawdzić kod TableAdapter s `Connection` właściwości. Jak wspomniano w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczek, możemy wyświetlić automatycznie wygenerowany kod TableAdapter, przechodząc do okna widoku klasy, przechodzenie do odpowiedniej klasy, a następnie klikając dwukrotnie nazwę elementu członkowskiego.

Przejdź do widoku klasy okna, przechodząc do menu Widok i wybieranie widoku klasy (lub wpisując polecenie Ctrl + Shift + C). W górnej połowie okno widoku klasy Drąż w dół do `NorthwindTableAdapters` przestrzeni nazw i wybierz `ProductsTableAdapter` klasy. Spowoduje to wyświetlenie `ProductsTableAdapter` s członków w dolnej połowie widoku klasy, jak pokazano na rysunku 2. Kliknij dwukrotnie `Connection` właściwości, aby wyświetlić jego kod.


![Kliknij dwukrotnie właściwość połączenia w widoku klas, aby wyświetlić jego automatycznego generowania kodu](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Rysunek 2**: kliknij dwukrotnie właściwość połączenia w widoku klas, aby wyświetlić jego automatycznego generowania kodu


TableAdapter s `Connection` właściwości i innych następujące związane z połączeniem kodu:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Podczas tworzenia wystąpienia klasy TableAdapter, zmiennej członkowskiej `_connection` jest równa `Nothing`. Gdy `Connection` dostępu do właściwości, najpierw sprawdza, czy `_connection` zmiennej członkowskiej zostały utworzone. Jeśli nie, `InitConnection` wywoływana jest metoda, która tworzy `_connection` i ustawia jej `ConnectionString` właściwości do wartości ciągu połączenia z pierwszym krokiem s Kreatora konfiguracji TableAdapter.

`Connection` Właściwości można również przypisać do `SqlConnection` obiektu. Dzięki temu kojarzy nowe `SqlConnection` obiektu z każdym TableAdapter s `SqlCommand` obiektów.

## <a name="step-2-exposing-connection-level-settings"></a>Krok 2: Udostępnianie ustawienia na poziomie połączenia

Informacje o połączeniu należy pozostają hermetyzowany w TableAdapter i nie być dostępna dla innych warstwy architektury aplikacji. Jednak mogą istnieć scenariusze podczas informacji poziomem połączenia s TableAdapter musi być dostępny lub można dostosować do kwerendy, użytkownika lub strony ASP.NET.

Let s rozszerzyć `ProductsTableAdapter` w `Northwind` zestawu danych, aby uwzględnić `ConnectionString` właściwość, która może posłużyć warstwy logiki biznesowej do odczytu lub zmień parametry połączenia używane przez TableAdapter.

> [!NOTE]
> A *ciąg połączenia* jest ciągiem, który określa informacje dotyczące połączenia bazy danych, takiego jak dostawca, aby użyć lokalizacji bazy danych, poświadczenia uwierzytelniania i innych ustawień związanych z bazy danych. Lista wzorców ciąg połączenia używany przez różnych dostawców i magazyny danych, zobacz [ConnectionStrings.com](http://www.connectionstrings.com/).


Zgodnie z opisem w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczka wpisane zestawu danych s automatycznie generowanej klasy można rozszerzyć za pomocą klasy częściowe. Najpierw utwórz nowy podfolder w projekcie o nazwie `ConnectionAndCommandSettings` pod `~/App_Code/DAL` folderu.


![Dodaj podfolder o nazwie ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Rysunek 3**: Dodaj podfolder o nazwie `ConnectionAndCommandSettings`


Dodaj nowy plik klasy o nazwie `ProductsTableAdapter.ConnectionAndCommandSettings.vb` , a następnie wprowadź poniższy kod:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Ta klasa częściowe dodaje `Public` właściwości o nazwie `ConnectionString` do `ProductsTableAdapter` klasy, która umożliwia wszystkie warstwy odczytać lub zaktualizować parametry połączenia dla s TableAdapter połączenie znajdujące się poniżej.

Z tą klasą częściowe utworzony (i zapisany), otwórz `ProductsBLL` klasy. Przejdź do jednego z istniejących metod i wpisz `Adapter` , a następnie naciśnij klawisz okresu można wyświetlić IntelliSense. Powinien zostać wyświetlony nowy `ConnectionString` właściwość jest dostępna w IntelliSense, co oznacza, że można programowo odczytu lub Dostosuj wartość tego parametru logiki warstwy Biznesowej.

## <a name="exposing-the-entire-connection-object"></a>Udostępnianie obiektu całego połączenia

Ta klasa częściowe przedstawia tylko jedną właściwość obiektu podstawowego połączenia: `ConnectionString`. Jeśli chcesz udostępnić poza granicach TableAdapter obiektu całego połączenia, można również zmienić `Connection` poziom ochrony właściwości s. Automatycznie wygenerowany kod, możemy się zbadana w kroku 1 wykazało TableAdapter s `Connection` właściwość jest oznaczona jako `Friend`, co oznacza, że jego można uzyskać tylko przez klasy w tym samym zestawie. Można to zmienić, jednak za pomocą TableAdapter s `ConnectionModifier` właściwości.

Otwórz `Northwind` zestawu danych, kliknij przycisk `ProductsTableAdatper` w projektancie, a następnie przejdź do okna właściwości. Brak zobaczysz `ConnectionModifier` ustawioną wartość domyślną `Assembly`. Aby `Connection` dostępne spoza zestawu s wpisane zestawu danych, zmień właściwość `ConnectionModifier` właściwości `Public`.


[![Poziom dostępności s właściwości połączenia można skonfigurować za pomocą właściwości ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Rysunek 4**: `Connection` właściwości s ułatwień dostępu poziomu można skonfigurować za pomocą `ConnectionModifier` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Zapisywanie zestawu danych, a następnie wróć do `ProductsBLL` klasy. Zanim, przejdź do jednego z istniejących metod i wpisz `Adapter` , a następnie naciśnij klawisz okresu można wyświetlić IntelliSense. Lista powinna zawierać `Connection` właściwości, co oznacza, że można teraz programowo odczytu lub przypisać wszystkie ustawienia połączenia poziomu z logiki warstwy Biznesowej.

## <a name="step-3-examining-the-command-related-properties"></a>Krok 3: Sprawdzenie właściwości powiązane polecenia

TableAdapter składa się z główne zapytanie, które domyślnie został wygenerowany automatycznie `INSERT`, `UPDATE`, i `DELETE` instrukcje. To zapytanie głównego s `INSERT`, `UPDATE`, i `DELETE` instrukcje zaimplementowano w kodzie s TableAdapter jako obiekt karty danych ADO.NET za pośrednictwem `Adapter` właściwości. Jak jego `Connection` właściwość `Adapter` typ danych właściwości s jest określany przez dostawcę danych używane. Ponieważ te samouczki użyć dostawca SqlClient `Adapter` właściwość jest typu [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapter s `Adapter` właściwość ma trzy właściwości typu `SqlCommand` używaną do problem `INSERT`, `UPDATE`, i `DELETE` instrukcji:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A `SqlCommand` obiektu jest odpowiedzialny za wysyłanie określonej kwerendy w bazie danych i ma właściwości, takie jak: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), który zawiera ad hoc instrukcji SQL lub procedurę składowaną można wykonać; i [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), która jest kolekcją `SqlParameter` obiektów. Jak widzieliśmy w [tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczek, te polecenia obiekty można dostosować za pomocą okna właściwości.

Oprócz jej główne zapytanie TableAdapter mogą obejmować zmienną liczbę metod, gdy została wywołana, wysyłania określonego polecenia w bazie danych. Obiekt polecenia główne zapytanie s i obiekty poleceń dla wszystkich dodatkowych metod są przechowywane w TableAdapter s `CommandCollection` właściwości.

Let s Poświęć chwilę, aby przyjrzeć się kod wygenerowany przez `ProductsTableAdapter` w `Northwind` zestawu danych dla tych dwóch właściwości i ich obsługi zmiennych Członkowskich i metody pomocnicze:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Kod `Adapter` i `CommandCollection` właściwości ściśle elementu, którego `Connection` właściwości. Istnieją zmienne Członkowskie, zawierających obiekty używane przez właściwości. Właściwości `Get` metod dostępu do uruchomienia przez sprawdzenie, czy jest odpowiedni zmiennej członkowskiej `Nothing`. Jeśli tak, metodę inicjalizacji jest wywoływana, która tworzy wystąpienie zmiennej członkowskiej i przypisuje podstawowe właściwości powiązanych z polecenia.

## <a name="step-4-exposing-command-level-settings"></a>Krok 4: Udostępnianie ustawienia na poziomie polecenia

Najlepiej, jeśli informacje o poziomie polecenia powinny pozostać hermetyzowany w warstwie dostępu do danych. Te informacje należy potrzebna w innych warstwy architektury, jednak go można uwidocznić przez klasę częściową podobnie jak przy użyciu ustawień poziomem połączenia.

Ponieważ TableAdapter ma tylko jedną `Connection` właściwość Kod udostępnianie ustawień poziom połączeń jest bardzo prosta. Elementy są nieco bardziej skomplikowane podczas modyfikowania ustawień na poziomie polecenia, ponieważ TableAdapter może mieć wielu obiektów polecenia - `InsertCommand`, `UpdateCommand`, i `DeleteCommand`, wraz ze zmienną liczbą obiektów polecenia w `CommandCollection` Właściwość. Podczas aktualizowania ustawień na poziomie polecenia, te ustawienia należy propagowane do wszystkich obiektów polecenia.

Na przykład załóżmy, że wystąpiły niektórych zapytań w TableAdapter, który miał nadzwyczajne dużo czasu na wykonanie. Wykonaj jedną z tych zapytań za pomocą TableAdapter, chcemy może zwiększyć obiektu polecenia s [ `CommandTimeout` właściwości](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Ta właściwość określa liczbę sekund oczekiwania na wykonanie polecenia i domyślnie ustawiany na 30.

Aby umożliwić `CommandTimeout` właściwość zostanie skorygowany logiki warstwy Biznesowej, Dodaj następujący `Public` metodę `ProductsDataTable` przy użyciu pliku klasy częściowej utworzony w kroku 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Tej metody można wywołać z logiki warstwy Biznesowej lub warstwy prezentacji, aby ustawić limit czasu polecenia dotyczące wszystkich problemów z poleceń przez to wystąpienie TableAdapter.

> [!NOTE]
> `Adapter` i `CommandCollection` właściwości są oznaczone jako `Private`, co oznacza, aby były dostępne tylko z kod w metodzie TableAdapter. W odróżnieniu od `Connection` właściwości, te modyfikatorów dostępu nie są konfigurowalne. W związku z tym, jeśli należy do udostępnienia właściwości na poziomie polecenia do innych warstw w architekturze należy użyć metody częściowej klasy opisanych wyżej w celu zapewnienia `Public` metody lub właściwości, która zapisuje lub odczytuje `Private` polecenia obiektów.


## <a name="summary"></a>Podsumowanie

TableAdapters w elemencie DataSet wpisane służyć Hermetyzowanie szczegółów dostępu do danych i złożoności. Przy użyciu TableAdapters, firma Microsoft nie trzeba martwić pisanie kodu ADO.NET do łączenia z bazą danych, wydać polecenie lub wypełnić wyniki do elementu DataTable. Jest wszystkie obsługiwane automatycznie firmie Microsoft.

Jednak może być razy podczas musimy dostosować niskiego poziomu charakterystykę ADO.NET, np. zmienić parametry połączenia lub domyślnej wartości limitu czasu połączenia lub polecenie. TableAdapter został wygenerowany automatycznie `Connection`, `Adapter`, i `CommandCollection` właściwości, ale te są albo `Friend` lub `Private`, domyślnie. Rozszerzając TableAdapter do uwzględnienia przy użyciu klasy częściowe można uwidocznić tej informacji wewnętrznych `Public` metody lub właściwości. Alternatywnie s TableAdapter `Connection` właściwości modyfikator dostępu można skonfigurować za pomocą TableAdapter s `ConnectionModifier` właściwości.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Burnadette Leigh, ren S Lauritsen Jacoba Teresa Murphy i Hilton Geisenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](working-with-computed-columns-vb.md)
> [dalej](protecting-connection-strings-and-other-configuration-information-vb.md)
