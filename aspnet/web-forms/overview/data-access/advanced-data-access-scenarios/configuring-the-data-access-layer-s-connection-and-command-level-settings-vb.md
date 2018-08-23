---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Konfigurowanie ustawienia na poziomie połączenia i poleceń warstwy dostępu do danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: TableAdapter w zestawie danych wpisane automatycznie zajmie się z bazą danych, wydawanie polecenia i wypełnianie DataTable z wynikami...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: d44372ef3eaf7634d3bf3a82bd2c1eb1d710f786
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753111"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Konfigurowanie ustawienia na poziomie połączenia i poleceń warstwy dostępu do danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) lub [Pobierz plik PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> TableAdapter w zestawie danych wpisane automatycznie zajmie się z bazą danych, wydawanie polecenia i wypełnianie obiektu DataTable z wyników. Istnieją sytuacje, ale jeśli chcemy zajmie się tymi szczegółowymi informacjami, osoby, a w tym samouczku będziemy Dowiedz się, jak uzyskać dostęp do ustawień połączenia i polecenia poziomu bazy danych w metodzie TableAdapter.


## <a name="introduction"></a>Wprowadzenie

W tej serii samouczka użyliśmy wpisanych zestawów danych do zaimplementowania warstwy dostępu do danych i obiektów biznesowych w naszej architektury warstwowej. Zgodnie z opisem w [pierwszego samouczka dotyczącego](../introduction/creating-a-data-access-layer-vb.md), s wpisany zestaw danych DataTable pełnią repozytoria danych elementów TableAdapter działać jako otoki komunikację z bazą danych, aby pobrać i zmodyfikować danych bazowych. Elementów TableAdapter hermetyzacji złożoność związaną z pracy z bazą danych i zapisuje nam z konieczności pisania kodu do połączenia z bazą danych, wydać polecenie lub wypełnić wyniki do elementu DataTable.

Brak sytuacji, gdy będziemy musieli burrow na głębokości TableAdapter i napisać kod, który współpracuje bezpośrednio z obiektów ADO.NET. W [opakowywanie modyfikacji bazy danych w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) samouczek, na przykład, dodaliśmy metody do TableAdapter początku, zatwierdzanie i wycofywanie transakcji ADO.NET. Te metody używane wewnętrznego, utworzone ręcznie `SqlTransaction` obiektu, który został przypisany do TableAdapter s `SqlCommand` obiektów.

W tym samouczku zostanie omówiony sposób uzyskać dostęp do ustawień połączenia i polecenia poziomu bazy danych w metodzie TableAdapter. W szczególności firma Microsoft doda funkcje `ProductsTableAdapter` , umożliwia dostęp do podstawowych parametrów połączenia i polecenia Ustawienia limitu czasu.

## <a name="working-with-data-using-adonet"></a>Praca z danymi za pomocą platformy ADO.NET

Microsoft .NET Framework zawiera mnóstwo klasy przeznaczone specjalnie do pracy z danymi. Te klasy można znaleźć w [ `System.Data` przestrzeni nazw](https://msdn.microsoft.com/library/system.data.aspx), są określane jako *ADO.NET* klasy. Niektóre z klas w obszarze parasola ADO.NET są powiązane z określonego *dostawcy danych*. Dostawca danych można traktować jako kanał komunikacyjny, który umożliwia informacje przesyłane między klasami ADO.NET i źródłowy magazyn danych. Brak uogólnionego dostawców, takich jak OLE DB i ODBC, a także dostawców, które są specjalnie przeznaczone dla konkretnej bazy danych systemu. Na przykład gdy jest możliwość nawiązywania połączeń z bazą danych programu Microsoft SQL Server przy użyciu dostawcy OleDb dostawcy SqlClient jest znacznie bardziej efektywne zgodnie z jego został zaprojektowany oraz zoptymalizowany specjalnie dla programu SQL Server.

Podczas programowego dostępu do danych, często służy następującego wzorca:

1. Nawiąż połączenie z bazą danych.
2. Wydać polecenie.
3. Aby uzyskać `SELECT` zapytań, pracować z wynikowego rekordami.

Istnieją osobne klasy ADO.NET do wykonania, każdy z tych kroków. Aby połączyć się z bazą danych przy użyciu dostawcy SqlClient, na przykład użyć [ `SqlConnection` klasy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Problem `INSERT`, `UPDATE`, `DELETE`, lub `SELECT` polecenia w bazie danych, użyj [ `SqlCommand` klasy](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Z wyjątkiem [opakowywanie modyfikacji bazy danych w ramach transakcji](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) samouczek, nie mamy pisać kodu ADO.NET niskiego poziomu, osoby, ponieważ kod wygenerowany automatycznie TableAdapters zawiera funkcje niezbędne do połączenia z bazą danych, wydawać polecenia, pobieranie danych i wypełniania tych danych do DataTable. Jednak może być czasy, kiedy musimy dostosować te ustawienia niskiego poziomu. W ciągu następnych kilku krokach będziemy sprawdzać jak korzystać z obiektów ADO.NET używane wewnętrznie elementów TableAdapter.

## <a name="step-1-examining-with-the-connection-property"></a>Krok 1: Badanie przy użyciu właściwości połączenia

Każda klasa TableAdapter ma `Connection` właściwość, która określa informacje o połączeniu z bazą danych. Ten typ danych właściwości s i `ConnectionString` wartości są określane przez wybrane w Kreatorze konfiguracji TableAdapter. Pamiętaj, że po dodaniu najpierw TableAdapter z zestawem danych wpisane ten kreator zapyta, nam dla bazy danych źródła (patrz rysunek 1). Listy rozwijanej w pierwszym kroku dotyczy tych baz danych, określone w pliku konfiguracji, a także innych baz danych w Eksploratorze serwera s połączeń danych. Jeśli bazy danych, które firma Microsoft nie istnieje na liście rozwijanej, można określić nowego połączenia z bazą danych, klikając przycisk nowe połączenie oraz udostępniając informacje o połączeniu potrzebne.


[![Pierwszym krokiem, który Kreator konfiguracji TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Rysunek 1**: pierwszy krok w Kreatorze konfiguracji TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


Umożliwiają s Poświęć chwilę, aby sprawdzić kod TableAdapter s `Connection` właściwości. Jak wspomniano w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczek, możemy wyświetlić automatycznie wygenerowany kod TableAdapter, przechodząc do okna widoku klasy, wyświetlających odpowiedniej klasy, a następnie klikając dwukrotnie nazwę elementu członkowskiego.

Przejdź do okna widoku klasy, przechodząc do menu widoku i wybierając pozycję Widok klas (lub wpisując Ctrl + Shift + C). W górnej połowie okna widoku klasy Drąż w dół do `NorthwindTableAdapters` przestrzeni nazw i wybierz `ProductsTableAdapter` klasy. Spowoduje to wyświetlenie `ProductsTableAdapter` s członków w dolnej części widoku klas, jak pokazano na rysunku 2. Kliknij dwukrotnie `Connection` właściwości, aby wyświetlić jej kod.


![Kliknij dwukrotnie właściwości połączenia w widoku klas, aby wyświetlić jego automatycznego generowania kodu](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Rysunek 2**: kliknij dwukrotnie właściwości połączenia w widoku klas, aby wyświetlić jego automatycznego generowania kodu


TableAdapter s `Connection` właściwości i inne związane z połączeniem kod poniżej:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Podczas tworzenia wystąpienia klasy TableAdapter, zmiennej składowej `_connection` jest równa `Nothing`. Gdy `Connection` dostęp do właściwości, najpierw sprawdza, czy `_connection` utworzono wystąpienia zmiennej elementu członkowskiego. Jeśli nie, `InitConnection` wywoływana jest metoda, która tworzy wystąpienie `_connection` i ustawia jego `ConnectionString` właściwości na wartość parametrów połączenia, które są określone w kroku pierwszym kreatora s konfiguracji TableAdapter.

`Connection` Właściwość można przypisać do `SqlConnection` obiektu. Ten sposób powoduje skojarzenie nowego `SqlConnection` obiektu z każdym TableAdapter s `SqlCommand` obiektów.

## <a name="step-2-exposing-connection-level-settings"></a>Krok 2: Udostępnianie ustawień na poziomie połączenia

Informacje o połączeniu należy pozostają hermetyzowany w terminie TableAdapter i nie być dostępna dla innych warstw architektury aplikacji. Jednak możliwe scenariusze podczas informacji z poziomu połączenia s TableAdapter musi być dostępny lub można dostosować do kwerendy, użytkownika lub strony ASP.NET.

Pozwól s rozszerzyć `ProductsTableAdapter` w `Northwind` zestawu danych, aby uwzględnić `ConnectionString` właściwość, która może służyć przez warstwę logiki biznesowej do odczytu lub zmień parametry połączenia używane przez TableAdapter.

> [!NOTE]
> A *parametry połączenia* jest ciągiem, który określa informacje o połączeniu bazy danych, takiego jak dostawca, aby użyć lokalizacji bazy danych, poświadczenia uwierzytelniania i innych ustawień związanych z bazy danych. Aby uzyskać listę wzorców ciąg połączenia używany przez wiele magazynów danych i dostawców, zobacz [ConnectionStrings.com](http://www.connectionstrings.com/).


Zgodnie z opisem w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczku klasy generowanych automatycznie s wpisana zestawu danych można rozszerzyć za pomocą klasy częściowe. Najpierw utwórz nowy podfolder w projekcie o nazwie `ConnectionAndCommandSettings` poniżej `~/App_Code/DAL` folderu.


![Dodaj podfolder o nazwie ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Rysunek 3**: Dodaj podfolder o nazwie `ConnectionAndCommandSettings`


Dodaj plik klasy o nazwie `ProductsTableAdapter.ConnectionAndCommandSettings.vb` i wprowadź następujący kod:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Ta klasa częściowa dodaje `Public` właściwość o nazwie `ConnectionString` do `ProductsTableAdapter` klasę, która zezwala na wszystkie warstwy, aby odczytać lub zaktualizować parametry połączenia dla s TableAdapter połączenie znajdujące się poniżej.

Za pomocą tej klasy częściowej utworzony i zapisany, otwórz `ProductsBLL` klasy. Przejdź do jednej z istniejących metod i wpisać `Adapter` , a następnie naciśnij klucz okresu, aby wywołać funkcję IntelliSense. Zostanie wyświetlone nowe `ConnectionString` właściwość jest dostępna w technologii IntelliSense, co oznacza, czy można programowo odczytu lub dostosowanie tej wartości od LOGIKI.

## <a name="exposing-the-entire-connection-object"></a>Udostępnianie obiektów całego połączenia

Ta klasa częściowa udostępnia tylko jedną właściwość obiektu bazowego połączenia: `ConnectionString`. Jeśli chcesz udostępnić obiektu całego połączenia po przekroczeniu granicach TableAdapter, można również zmienić `Connection` poziom ochrony właściwości s. Automatycznie wygenerowany kod, zbadaliśmy w kroku 1 wykazało, że TableAdapter s `Connection` właściwość jest oznaczona jako `Friend`, co oznacza, że jej może zostać oceniony jedynie przez klasy z tego samego zestawu. Można to zmienić, jednak za pośrednictwem TableAdapter s `ConnectionModifier` właściwości.

Otwórz `Northwind` zestawu danych, kliknij pozycję `ProductsTableAdatper` w projektancie, a następnie przejdź do okna właściwości. Zobaczysz `ConnectionModifier` ustawiony na wartość domyślną `Assembly`. Aby `Connection` dostępne spoza zestawu s wpisana zestawu danych, zmień właściwość `ConnectionModifier` właściwość `Public`.


[![Poziom dostępności s właściwości połączenia można skonfigurować za pomocą właściwości ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Rysunek 4**: `Connection` właściwości ułatwień dostępu można skonfigurować poziom s za pośrednictwem `ConnectionModifier` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


Zapisz zestaw danych, a następnie wróć do `ProductsBLL` klasy. Jak wcześniej, przejdź do jednej z istniejących metod i wpisz w polu `Adapter` , a następnie naciśnij klucz okresu, aby wywołać funkcję IntelliSense. Lista powinna zawierać `Connection` właściwości, co oznacza, że można teraz programowo odczytu lub przypisać wszystkie ustawienia na poziomie połączenia od LOGIKI.

## <a name="step-3-examining-the-command-related-properties"></a>Krok 3: Sprawdzenie właściwości związane z polecenia

TableAdapter składa się z główne zapytanie, która domyślnie jest generowany automatycznie `INSERT`, `UPDATE`, i `DELETE` instrukcji. To główne zapytanie s `INSERT`, `UPDATE`, i `DELETE` instrukcje są implementowane w kodzie s TableAdapter jako obiekt karty danych ADO.NET za pośrednictwem `Adapter` właściwości. Za pomocą jego `Connection` właściwości `Adapter` typ danych właściwości s jest określany przez dostawcę danych używane. Ponieważ te samouczki używa dostawcy SqlClient `Adapter` właściwość jest typu [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

TableAdapter s `Adapter` właściwość ma trzy właściwości typu `SqlCommand` używaną do problemu `INSERT`, `UPDATE`, i `DELETE` instrukcji:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A `SqlCommand` obiektu jest odpowiedzialny za wysyłanie określone zapytanie do bazy danych i ma właściwości, takie jak: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), który zawiera instrukcję SQL w trybie ad-hoc lub procedura składowana do wykonania; i [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), który jest kolekcją `SqlParameter` obiektów. Jak widzieliśmy w [Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md) samouczek, te polecenia obiekty można dostosować w oknie właściwości.

Oprócz jego główne zapytanie TableAdapter może zawierać zmienną liczbę metod, gdy wywoływany, wysyłania określonego polecenia w bazie danych. Obiekt polecenia główne zapytanie s i obiekty poleceń dla wszystkich dodatkowych metod, które są przechowywane w TableAdapter s `CommandCollection` właściwości.

Umożliwiają s Poświęć chwilę, aby przyjrzeć się kod wygenerowany przez `ProductsTableAdapter` w `Northwind` zestawu danych, te dwie właściwości i ich obsługi zmiennych składowych i metody pomocnicze:


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

Kod `Adapter` i `CommandCollection` właściwości ścisłe naśladowanie z `Connection` właściwości. Brak zmiennych składowych, używane do przechowywania używanych przez właściwości obiektów. Właściwości `Get` Akcesory początek sprawdzanie, czy odpowiednią zmienną elementu członkowskiego `Nothing`. Jeśli tak, metodę inicjalizacji jest wywoływana, który tworzy wystąpienie zmiennej elementu członkowskiego i przypisuje podstawowe właściwości powiązanych z polecenia.

## <a name="step-4-exposing-command-level-settings"></a>Krok 4: Udostępnianie ustawień na poziomie polecenia

W idealnym przypadku informacji z poziomu polecenia powinny pozostać zhermetyzowanych w ramach warstwy dostępu do danych. Powinien to być potrzebne informacje w innych warstwach architektury jednak można ją wyeksponować za pośrednictwem klasy częściowej, podobnie jak przy użyciu ustawień poziom połączenia.

Ponieważ TableAdapter ma tylko jeden `Connection` właściwość udostępnia ustawienia na poziomie połączenia kod jest dość prosta. Elementy są nieco bardziej skomplikowane, podczas modyfikowania ustawień na poziomie polecenia, ponieważ TableAdapter może mieć wiele obiektów polecenia - `InsertCommand`, `UpdateCommand`, i `DeleteCommand`, wraz z różną liczbą obiektów polecenia w `CommandCollection` Właściwość. Podczas aktualizowania ustawień na poziomie polecenia, te ustawienia należy są propagowane do wszystkich obiektów polecenia.

Na przykład Wyobraź sobie, że były niektórych zapytań w TableAdapter, który zajęło nadzwyczajne, długi czas do wykonania. Wykonaj jedną z tych zapytań za pomocą TableAdapter, możemy być może warto zwiększyć obiektu polecenia s [ `CommandTimeout` właściwość](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Ta właściwość określa liczbę sekund oczekiwania na wykonanie polecenia, a wartość domyślna to 30.

Aby umożliwić `CommandTimeout` właściwość zostanie skorygowany LOGIKI, Dodaj następujący kod `Public` metody `ProductsDataTable` przy użyciu pliku częściowej klasy utworzone w kroku 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Tej metody można wywołać z LOGIKI lub Warstwa prezentacji, aby ustawić limit czasu polecenia dotyczące wszystkich problemów z poleceń, przez to wystąpienie TableAdapter.

> [!NOTE]
> `Adapter` i `CommandCollection` właściwości są oznaczone jako `Private`, co oznacza, są dostępne tylko w kodzie w metodzie TableAdapter. W odróżnieniu od `Connection` właściwości tych modyfikatorów dostępu nie są konfigurowalne. W związku z tym, jeśli musisz ujawnić właściwości na poziomie polecenia do innych warstw architektury należy użyć metody częściowej klasy omówione powyżej, aby zapewnić `Public` metody lub właściwości, który odczytuje lub zapisuje `Private` polecenia obiektów.


## <a name="summary"></a>Podsumowanie

TableAdapter w zestawie danych wpisane służą do hermetyzacji szczegółów dostępu do danych i złożoności. Za pomocą adapterów TableAdapter, nie musimy martwić się o pisaniu kodu ADO.NET do łączenia z bazą danych, wydać polecenie lub wypełnić wyniki do elementu DataTable. Jego wszystkie odbywa się automatycznie dla nas.

Jednak może być czasy, kiedy musimy dostosować zdiagnozowania ADO.NET niskiego poziomu, takie jak zmiana parametrów połączenia lub domyślne wartości limitu czasu połączenia lub polecenia. TableAdapter został wygenerowany automatycznie `Connection`, `Adapter`, i `CommandCollection` właściwości, ale te są albo `Friend` lub `Private`, domyślnie. Można ujawnić te informacje wewnętrzne, rozszerzając TableAdapter przy użyciu klas częściowych do dołączenia `Public` metody lub właściwości. Alternatywnie s TableAdapter `Connection` modyfikator dostępu właściwości można skonfigurować za pomocą TableAdapter s `ConnectionModifier` właściwości.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Burnadette Leigh, S ren Jacob Lauritsen Teresa Murphy i Hilton Geisenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](working-with-computed-columns-vb.md)
> [dalej](protecting-connection-strings-and-other-configuration-information-vb.md)
