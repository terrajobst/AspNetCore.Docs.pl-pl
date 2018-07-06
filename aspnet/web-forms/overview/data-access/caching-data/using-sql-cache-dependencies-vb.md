---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Używanie zależności pamięci podręcznej SQL (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Najprostsza strategii buforowania jest umożliwienie dane w pamięci podręcznej wygasa po upływie określonego czasu. Jednak takie proste podejście oznacza, że maintai dane w pamięci podręcznej...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 74692fb7018cd75e29afc6d5852caddfdac1ed06
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379848"
---
<a name="using-sql-cache-dependencies-vb"></a>Używanie zależności pamięci podręcznej SQL (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) lub [Pobierz plik PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> Najprostsza strategii buforowania jest umożliwienie dane w pamięci podręcznej wygasa po upływie określonego czasu. Ale to proste podejście oznacza, że dane w pamięci podręcznej przechowuje żaden związek z jego źródle danych, co nieaktualnych danych, które odbywa się za długa lub bieżących danych, który wygasł zbyt szybko. Lepszym rozwiązaniem jest korzystanie z klasy SqlCacheDependency, tak aby danych pozostaje w pamięci podręcznej do momentu jego danych źródłowych został zmodyfikowany w bazie danych SQL. Ten samouczek pokazuje sposób.


## <a name="introduction"></a>Wprowadzenie

Techniki buforowania badany w [buforowanie danych za pomocą kontrolki ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) i [buforowania danych w architekturze](caching-data-in-the-architecture-vb.md) samouczki umożliwia wykluczenie danych z pamięci podręcznej po określonym na podstawie czasu wygaśnięcia okres. To podejście jest najprostszym sposobem równoważyć wzrosła wydajność takich buforowania względem nieaktualność danych. Wybierając czas wygaśnięcia *x* (w sekundach), deweloper strony concedes do korzystania z zalet wydajności buforowanie tylko *x* (w sekundach), ale umieścić proste czy jej dane nigdy nie będą nieaktualne dłuższy niż maksimum z *x* sekund. Oczywiście w przypadku danych statycznych *x* może zostać rozszerzony do cyklu życia aplikacji sieci web, jak zostały sprawdzone w [buforowanie danych przy uruchamianiu aplikacji](caching-data-at-application-startup-vb.md) samouczka.

Podczas buforowania danych bazy danych, na podstawie czasu wygaśnięcia często jest wybierany w celu ułatwienia jej, ale często jest niewystarczająca rozwiązaniem. W idealnym przypadku danych w bazie danych może pozostać w pamięci podręcznej aż danych bazowych został zmodyfikowany w bazie danych. dopiero wtedy będzie można wykluczyć pamięci podręcznej. To podejście zapewnia korzyści w zakresie wydajności buforowania i minimalizuje czas trwania nieaktualnych danych. Jednakże aby móc korzystać z tych korzyści, musi być system w miejscu, który wie, kiedy podstawowych danych w bazie danych została zmodyfikowana i wyklucza mogą odpowiednie elementy z pamięci podręcznej. Przed ASP.NET 2.0 programistom stron były odpowiedzialne za wykonanie tego systemu.

Program ASP.NET 2.0 zapewnia [ `SqlCacheDependency` klasy](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) i infrastruktury wymaganej do określenia, kiedy nastąpiła zmiana w bazie danych, aby odpowiednie pamięci podręcznej elementów może zostać wykluczony. Istnieją dwie techniki do określania, kiedy zmienił danych bazowych: powiadomienie i sondowania. Po omówieniu różnice między powiadomień, jak i sondowanie, utworzymy infrastruktury niezbędnych do obsługi sondowania i zobacz, jak możesz używać `SqlCacheDependency` klasy w deklaracyjne i programowe scenariuszy.

## <a name="understanding-notification-and-polling"></a>Opis powiadomienia i sondowania

Istnieją dwie techniki, które mogą służyć do określenia, kiedy dane w bazie danych zostały zmodyfikowane: powiadomienie i sondowania. Z zastrzeżeniem bazy danych automatycznie powiadamia środowiska uruchomieniowego programu ASP.NET, gdy wyniki zapytania określonego zostały zmienione od czasu ostatniego wykonania zapytania, w tym momencie obrazuje elementy pamięci podręcznej skojarzonej z zapytaniem. Za pomocą sondowania, serwer bazy danych przechowuje informacje o kiedy konkretne tabele mają miejsce ostatnia aktualizacja. Środowisko uruchomieniowe ASP.NET okresowo sonduje bazy danych, aby sprawdzić, jakie tabele zostały zmienione od momentu ich wprowadzenia do pamięci podręcznej. Te tabele, których dane zostały zmodyfikowane ma ich elementów pamięci podręcznej skojarzonych wykluczona.

Opcja powiadomienie wymaga mniej konfiguracji niż sondowania i jest bardziej szczegółowego, ponieważ śledzi zmiany na poziomie zapytania, a nie na poziomie tabeli. Niestety powiadomienia są dostępne tylko w pełne wersje programu Microsoft SQL Server 2005 (tj. wersje non-Express). Jednak opcja sondowania może służyć dla wszystkich wersji programu Microsoft SQL Server w wersji 7.0 do 2005. Ponieważ te samouczki używać wydaniu Express programu SQL Server 2005, skupimy się na konfigurowaniu i przy użyciu opcji sondowania. Na końcu niniejszego samouczka, aby dalsze zasoby przy użyciu funkcji programu SQL Server 2005 s powiadomień, zapoznaj się w sekcji dalszego czytania.

Za pomocą sondowania, baza danych musi być skonfigurowany do zawierać tabelę o nazwie `AspNet_SqlCacheTablesForChangeNotification` zawierający trzy kolumny — `tableName`, `notificationCreated`, i `changeId`. Ta tabela zawiera wiersz dla każdej tabeli, która zawiera dane, w których konieczna może być używany w pamięci podręcznej zależności SQL w aplikacji sieci web. `tableName` Kolumna określa nazwę tabeli podczas `notificationCreated` określa datę i godzinę wiersz został dodany do tabeli. `changeId` Kolumna jest kolumną typu `int` i ma początkowa wartość 0. Jego wartość jest zwiększany po każdej modyfikacji do tabeli.

Oprócz `AspNet_SqlCacheTablesForChangeNotification` tabeli bazy danych również musi zawierać Wyzwalacze w każdej z tabel, które mogą się pojawić w pamięci podręcznej zależność SQL. Te wyzwalacze są wykonywane zawsze, gdy wstawione, zaktualizowane lub usunięte wiersz i zwiększ tabeli s `changeId` wartość w `AspNet_SqlCacheTablesForChangeNotification`.

Środowisko uruchomieniowe ASP.NET śledzi bieżącą `changeId` dla tabeli, gdy buforowanie danych przy użyciu `SqlCacheDependency` obiektu. Okresowo zaznaczono opcję bazy danych oraz wszelką `SqlCacheDependency` obiekty, których `changeId` różni się od wartości w bazie danych obrazuje od różniących się `changeId` wartość wskazuje, czy wprowadzono zmiany do tabeli, ponieważ dane był buforowany.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Krok 1: Poznawanie`aspnet_regsql.exe`wiersza polecenia

Dzięki podejściu sondowania bazy danych musi być skonfigurowany zawierać infrastruktury opisane powyżej: wstępnie zdefiniowanej tabeli (`AspNet_SqlCacheTablesForChangeNotification`), kilka procedur składowanych i wyzwalaczy w każdej z tabel, które mogą być używane w zależności pamięci podręcznej SQL w sieci web aplikacja. Tych tabel, procedur składowanych i wyzwalaczy można utworzyć za pomocą wiersza polecenia programu `aspnet_regsql.exe`, który znajduje się w `$WINDOWS$\Microsoft.NET\Framework\version` folderu. Aby utworzyć `AspNet_SqlCacheTablesForChangeNotification` tabeli i skojarzone procedur składowanych, uruchom następujące polecenie w wierszu polecenia:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Aby wykonać te polecenia logowania określona baza danych musi znajdować się w [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) i [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) ról. Aby zbadać języka T-SQL wysyłane do bazy danych przez `aspnet_regsql.exe` program wiersza polecenia, zobacz [ten wpis w blogu](http://scottonwriting.net/sowblog/posts/10709.aspx).


Na przykład, aby dodać infrastruktury do sondowania z bazą danych programu Microsoft SQL Server o nazwie `pubs` na serwerze bazy danych o nazwie `ScottsServer` przy użyciu uwierzytelniania Windows, przejdź do odpowiedniego katalogu i, w wierszu polecenia wpisz:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Po dodaniu infrastruktury poziomu bazy danych należy dodać wyzwalacze do tych tabel, które będą używane w zależności pamięci podręcznej SQL. Użyj `aspnet_regsql.exe` wiersza polecenia programu ponownie, ale określ za pomocą nazwy tabeli `-t` przełącznika i zamiast `-ed` Przełącz użyj `-et`, w następujący sposób:


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Aby dodać wyzwalaczy, aby `authors` i `titles` tabel na `pubs` bazy danych na `ScottsServer`, użyj:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

W tym samouczku dodane wyzwalaczy, aby `Products`, `Categories`, i `Suppliers` tabel. Omówimy składni konkretnego wiersza polecenia w kroku 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Krok 2: Odwoływanie się do programu Microsoft SQL Server 2005 Express Edition bazy danych w`App_Data`

`aspnet_regsql.exe` Wiersza polecenia programu wymaga nazwy bazy danych i serwera, aby można było dodać infrastruktury niezbędnych sondowania. Ale co to jest nazwa bazy danych i serwera dla programu Microsoft SQL Server 2005 Express bazy danych, która znajduje się w `App_Data` folder? Zamiast konieczności Odkryj, jakie są nazwy bazy danych i serwera, czy ve wykrył, że najprostszą metodą jest się można dołączyć bazy danych do `localhost\SQLExpress` bazy danych wystąpienia, a następnie zmień nazwę dane za pomocą [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Jeśli masz pełne wersje zainstalowane na komputerze programu SQL Server 2005, następnie prawdopodobnie masz już zainstalowany na tym komputerze program SQL Server Management Studio. Jeśli masz tylko wersji Express edition, możesz pobrać bezpłatną [programu Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Rozpocznij od zamknięcia programu Visual Studio. Następnie otwórz program SQL Server Management Studio i wybierz opcję nawiązać połączenie `localhost\SQLExpress` serwera przy użyciu uwierzytelniania Windows.


![Dołącz do localhost\SQLExpress serwera](using-sql-cache-dependencies-vb/_static/image1.gif)

**Rysunek 1**: dołączanie do `localhost\SQLExpress` serwera


Po nawiązaniu połączenia z serwerem Management Studio Pokaż serwera i podfoldery dla baz danych, zabezpieczeń i tak dalej. Kliknij prawym przyciskiem myszy na folder baz danych i wybierz opcję Dołącz. Zostanie wyświetlone okno dialogowe dołączanie bazy danych (zobacz rysunek 2). Kliknij przycisk Dodaj, a następnie wybierz pozycję `NORTHWND.MDF` folder bazy danych w sieci web aplikacji s `App_Data` folderu.


[![Dołącz NORTHWND. MDF bazy danych, z folderu App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Rysunek 2**: Dołącz `NORTHWND.MDF` bazy danych z `App_Data` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image2.png))


Spowoduje to dodanie bazy danych do folderu bazy danych. Nazwa bazy danych może być pełna ścieżka do pliku bazy danych lub pełną ścieżkę poprzedzonej ciągiem [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Aby uniknąć konieczności wpisywania w tej nazwie długich bazy danych, korzystając z aspnet\_regsql.exe narzędzia wiersza polecenia, Zmień nazwę dołączyć bazy danych do nazwy bardziej przyjaznego dla człowieka, klikając prawym przyciskiem myszy bazę danych po prostu i wybierając opcję zmiany nazwy. Czy mogę ve zmieniona Moja baza danych na DataTutorials.


![Zmień nazwę dołączonej bazie danych nazwę bardziej przyjaznego dla człowieka](using-sql-cache-dependencies-vb/_static/image3.gif)

**Rysunek 3**: Zmień nazwę dołączonej bazie danych nazwę bardziej przyjaznego dla człowieka


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Krok 3: Dodawanie infrastruktury sondowania z bazą danych Northwind

Teraz, gdy będziemy mieć dołączone `NORTHWND.MDF` bazy danych z `App_Data` folderu, możemy ponownie gotowe do dodania infrastruktury sondowania. Przy założeniu, że był zmianie bazy danych na DataTutorials, uruchom następujące polecenia, cztery:


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Po uruchomieniu tych poleceń cztery, kliknij prawym przyciskiem myszy nazwę bazy danych w programie Management Studio, przejdź do menu zadania, a następnie wybierz odłączania. Następnie zamknij program Management Studio i otwórz ponownie program Visual Studio.

Gdy program Visual Studio zawiera otwarte ponownie, należy przejść do bazy danych za pomocą Eksploratora serwera. Należy pamiętać, nowa tabela (`AspNet_SqlCacheTablesForChangeNotification`), nowy przechowywane procedury składowane i wyzwalacze na `Products`, `Categories`, i `Suppliers` tabel.


![Baza danych zawiera teraz infrastruktury niezbędnych sondowania](using-sql-cache-dependencies-vb/_static/image4.gif)

**Rysunek 4**: baza danych zawiera teraz infrastruktury niezbędnych sondowania


## <a name="step-4-configuring-the-polling-service"></a>Krok 4: Konfigurowanie usługi sondowania

Po utworzeniu wymagane tabele, wyzwalaczy i procedur składowanych w bazie danych, ostatnim krokiem jest skonfigurować usługi sondowania, w którym odbywa się za pośrednictwem `Web.config` , określając baz danych i częstotliwość sondowania (w milisekundach). Następujące znaczniki sonduje bazy danych Northwind, co sekundę.


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

`name` Wartość w `<add>` — element (NorthwindDB) kojarzy nazwę zrozumiałą dla użytkownika z określoną bazą danych. Podczas pracy z zależności pamięci podręcznej SQL, będziemy potrzebować do odwoływania się do nazwy bazy danych, zdefiniowane w tym miejscu oraz tabeli, w oparciu o dane w pamięci podręcznej. Zobaczymy, jak używać `SqlCacheDependency` klasy programowe kojarzenie zależności pamięci podręcznej SQL przy użyciu buforowanych danych w kroku 6.

Po ustanowieniu zależności pamięci podręcznej SQL, system sondowania połączy się z bazami danych zdefiniowanych w `<databases>` elementów co `pollTime` milisekund, a następnie wykonaj `AspNet_SqlCachePollingStoredProcedure` procedury składowanej. Tę procedurę składowaną — który został dodany z powrotem w kroku 3 przy użyciu `aspnet_regsql.exe` narzędzie wiersza polecenia — zwraca `tableName` i `changeId` wartości dla każdego rekordu w `AspNet_SqlCacheTablesForChangeNotification`. Nieaktualne zależności pamięci podręcznej SQL jest wykluczony z pamięci podręcznej.

`pollTime` Ustawienie wprowadza zależność między wydajnością a nieaktualność danych. Niewielki `pollTime` wartość zwiększa się liczba żądań w bazie danych, ale więcej szybko wyklucza mogą nieaktualnych danych z pamięci podręcznej. Większy `pollTime` wartość zmniejsza liczbę żądań bazy danych, ale zwiększa opóźnienia między po zmianie danych zaplecza, a kiedy obrazuje elementy pokrewne pamięci podręcznej. Na szczęście żądania bazy danych wykonuje prostą procedurę składowaną tego s zwracanie tylko kilka wierszy z tabeli proste, uproszczone. Ale eksperymentować z różnymi `pollTime` wartości, aby znaleźć idealne równowagę między bazą danych nieaktualność dostępu danych i aplikacji. Najmniejsza `pollTime` dozwolona wartość to 500.

> [!NOTE]
> Powyższy przykład zawiera pojedynczy `pollTime` wartość w `<sqlCacheDependency>` element, ale można opcjonalnie określić `pollTime` wartość w `<add>` elementu. Jest to przydatne, jeśli masz wiele określonych baz danych i chcesz dostosować częstotliwość sondowania na bazę danych.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Krok 5: Deklaratywne Praca z zależności pamięci podręcznej SQL

W krokach 1 – 4 przyjrzeliśmy się instrukcje konfiguracji infrastruktury niezbędnych bazy danych i konfiguracji systemu sondowania. Za pomocą tej infrastruktury w miejscu możemy teraz Dodaj elementy do pamięci podręcznej danych z skojarzone zależności pamięci podręcznej SQL przy użyciu techniki programistyczne lub deklaratywne. W tym kroku zajmiemy się, jak deklaratywne pracować z zależności pamięci podręcznej SQL. W kroku 6 przyjrzymy rozwiązania programowego.

[Buforowanie danych za pomocą kontrolki ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) samouczek zbadano deklaratywnych buforowania kontrolki ObjectDataSource. Wystarczy ustawić `EnableCaching` właściwości `True` i `CacheDuration` właściwość pewnego interwału czasu, kontrolki ObjectDataSource zostanie automatycznie w pamięci podręcznej danych zwracanych przez jego obiekt źródłowy dla określonego interwału. Kontrolki ObjectDataSource można również użyć co najmniej jeden zależności pamięci podręcznej SQL.

Aby zademonstrować, używanie zależności pamięci podręcznej SQL w sposób deklaratywny, otwórz `SqlCacheDependencies.aspx` stronie `Caching` folder i przeciągnij GridView z przybornika do projektanta. Ustaw GridView s `ID` do `ProductsDeclarative` i z jego tag inteligentny chcesz powiązać nowe kontrolki ObjectDataSource, o nazwie `ProductsDataSourceDeclarative`.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Rysunek 5**: Utwórz nowy o nazwie elementu ObjectDataSource `ProductsDataSourceDeclarative` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image4.png))


Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLL` klasy i zmień wartość na liście rozwijanej na karcie Wybierz `GetProducts()`. Na karcie aktualizacji, wybierz opcję `UpdateProduct` przeciążenia z trzema parametrami wejściowymi - `productName`, `unitPrice`, i `productID`. Ustaw list rozwijanych (Brak), znajdujące się na kartach INSERT i DELETE.


[![Użyj przeciążenia UpdateProduct trzech parametrów wejściowych](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Rysunek 6**: trzy parametry wejściowe za pomocą przeciążenia UpdateProduct ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image6.png))


[![Zmień wartość na liście rozwijanej na (Brak) do WSTAWIANIA i usuwania karty](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Rysunek 7**: Zmień wartość na liście rozwijanej na (Brak) dla Wstawianie i usuwanie kart ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image8.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio utworzy BoundFields i CheckBoxFields w widoku GridView dla każdego pola danych. Usuń wszystkie pola, ale `ProductName`, `CategoryName`, i `UnitPrice`i sformatuj te pola, zgodnie z potrzebami. Za pomocą tagu inteligentnego s GridView zaznacz pola wyboru włączone stronicowanie, włączyć sortowanie i Włącz edytowanie. Program Visual Studio ustawi ObjectDataSource s `OldValuesParameterFormatString` właściwość `original_{0}`. Aby zapewnić poprawne działanie funkcji edycji s GridView, albo Usuń tę właściwość całkowicie w składni deklaratywnej lub ustaw go z powrotem na jego wartość domyślną `{0}`.

Na koniec Dodaj kontrolkę etykieta Web powyżej GridView i ustaw jego `ID` właściwości `ODSEvents` i jego `EnableViewState` właściwość `False`. Po wprowadzeniu tych zmian, znaczniki deklaratywne s strony powinien wyglądać podobnie do poniższej. Należy pamiętać, ve wprowadzone liczby estetycznych dostosowań do pola kontrolki GridView, które nie są konieczne do zademonstrowania funkcji zależności pamięci podręcznej SQL.


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Następnie należy utworzyć program obsługi zdarzeń dla ObjectDataSource s `Selecting` zdarzeń i w jego Dodaj następujący kod:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Pamiętamy ObjectDataSource s `Selecting` zdarzeń jest uruchamiana tylko wtedy, gdy trwa pobieranie danych z jego obiekt źródłowy. Jeśli kontrolki ObjectDataSource uzyskuje dostęp do danych z własnej pamięci podręcznej, to zdarzenie nie jest uruchamiany.

Teraz odwiedź tę stronę za pośrednictwem przeglądarki. Ponieważ firma Microsoft ve jeszcze w celu implementacji wszelkich buforowania, każdym razem, strony, sortowanie lub edytowanie strony siatki powinien być wyświetlany tekst, wybranie event uruchamiane, jak pokazano na rysunku 8.


[![S ObjectDataSource wybranie Event uruchamia każdego czasu stronicowanej widoku GridView, edytować, lub posortowane](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Rysunek 8**: s ObjectDataSource `Selecting` czas każdej generowane zdarzenia jest stronicowanej widoku GridView, edytowana lub posortowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image10.png))


Jak widzieliśmy w [buforowanie danych za pomocą kontrolki ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) samouczek, ustawienie `EnableCaching` właściwości `True` powoduje, że ObjectDataSource do jego dane z pamięci podręcznej przez czas określony przez jego `CacheDuration` właściwości. Ma również kontrolki ObjectDataSource [ `SqlCacheDependency` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), która dodaje co najmniej jeden zależności pamięci podręcznej SQL buforowanych danych przy użyciu wzorca:


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Gdzie *databaseName* to nazwa bazy danych, jak to określono w `name` atrybutu `<add>` element `Web.config`, i *tableName* to nazwa tabeli bazy danych. Na przykład do utworzenia elementu ObjectDataSource, która przechowuje dane bezterminowo na podstawie zależności pamięci podręcznej SQL względem Northwind s `Products` tabeli, należy ustawić ObjectDataSource s `EnableCaching` właściwości `True` i jego `SqlCacheDependency` właściwości NorthwindDB:Products.

> [!NOTE]
> Możesz użyć zależności pamięci podręcznej SQL *i* na podstawie czasu wygaśnięcia, ustawiając `EnableCaching` do `True`, `CacheDuration` przedziału czasu i `SqlCacheDependency` do nazwy bazy danych i tabeli. Kontrolki ObjectDataSource Wyklucz swoje dane, po osiągnięciu na podstawie czasu wygaśnięcia lub gdy system sondowania zauważa, że danych bazowych bazy danych została zmieniona, zaznajomisz się stanie, najpierw.


GridView w `SqlCacheDependencies.aspx` wyświetla dane z dwóch tabel - `Products` i `Categories` (produkt s `CategoryName` pola są pobierane za pośrednictwem `JOIN` na `Categories`). W związku z tym, chcemy określić dwie zależności pamięci podręcznej SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Konfigurowanie kontrolki ObjectDataSource do obsługi pamięci podręcznej używanie zależności pamięci podręcznej SQL na temat produktów i kategorii](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Rysunek 9**: Konfigurowanie kontrolki ObjectDataSource do obsługi pamięci podręcznej przy użyciu pamięci podręcznej zależności SQL na `Products` i `Categories` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image12.png))


Po skonfigurowaniu ObjectDataSource do obsługi pamięci podręcznej, należy ponownie stronę za pośrednictwem przeglądarki. Ponownie zdarzenie wybranie tekstu, które są wywoływane powinny pojawić się na pierwszej wizyty strony, ale powinien zniknąć po stronicowania, sortowanie lub naciskać przycisków edycji, lub przycisk Anuluj. Jest to spowodowane po załadowaniu danych w pamięci podręcznej s ObjectDataSource nawet do momentu `Products` lub `Categories` tabele są modyfikowane lub dane są aktualizowane przy użyciu widoku GridView.

Po stronicowanie siatki i zapisując braku zdarzeń wybranie wyzwolone tekst, Otwórz nowe okno przeglądarki i przejdź do samouczka podstawy w edytowanie, wstawianie i usuwanie sekcji (`~/EditInsertDelete/Basics.aspx`). Zaktualizuj nazwę lub cena produktu. Następnie z pierwszego okna przeglądarki, Wyświetl innej strony danych, sortowania siatki lub kliknij przycisk Edytuj wiersz s. Tym razem wybranie zdarzenia wywoływane powinno się ponownie, podstawowej bazy danych, których dane zostały zmienione (zobacz rysunek 10). Jeśli tekst nie jest widoczny, poczekaj chwilę i spróbuj ponownie. Należy pamiętać, że usługa sondowania sprawdza zmiany `Products` tabeli co `pollTime` milisekund, więc istnieje opóźnienie między po zaktualizowaniu danych bazowych i kiedy zostanie wykluczony dane w pamięci podręcznej.


[![Modyfikowanie tabeli Produkty wyklucza mogą dane buforowane produktu](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Na rysunku nr 10**: Modyfikowanie tabeli Produkty wyklucza mogą danych produktu pamięci podręcznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Krok 6: Programowe Praca z`SqlCacheDependency`klasy

[Buforowania danych w architekturze](caching-data-in-the-architecture-vb.md) samouczek przyjrzano się korzyści z używania oddzielnych warstwy pamięci podręcznej w ramach architektury, w przeciwieństwie sprzęganiu pamięci podręcznej za pomocą kontrolki ObjectDataSource. W tym samouczku utworzyliśmy `ProductsCL` klasę wykazać programowo Praca z pamięci podręcznej danych. Używanie zależności pamięci podręcznej SQL w warstwie pamięci podręcznej, użyj `SqlCacheDependency` klasy.

W systemie sondowania `SqlCacheDependency` obiekt musi być skojarzone z określoną parą bazy danych i tabeli. Poniższy kod przykładowy tworzy `SqlCacheDependency` obiektu oparte na bazie danych Northwind s `Products` tabeli:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Wprowadź dwa parametry `SqlCacheDependency` Konstruktor s są odpowiednio nazwy bazy danych i tabeli. Za pomocą ObjectDataSource s `SqlCacheDependency` Właściwość Nazwa bazy danych, używane jest taka sama jak wartość określoną w `name` atrybutu `<add>` elementu w `Web.config`. Nazwa tabeli jest rzeczywista nazwa tabeli bazy danych.

Aby skojarzyć `SqlCacheDependency` za pomocą elementu dodane do pamięci podręcznej danych, użyj jednej z `Insert` przeciążenia metody, które akceptuje zależności. Poniższy kod dodaje *wartość* do pamięci podręcznej danych na nieokreślony czas, ale kojarzy ją z `SqlCacheDependency` na `Products` tabeli. Krótko mówiąc *wartość* pozostaną w pamięci podręcznej, dopóki nie zostanie usunięty z powodu ograniczeń pamięci, lub ponieważ sondowania system wykrył, że `Products` tabeli zmieniła się od jego był buforowany.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

S warstwy buforowania `ProductsCL` klasy obecnie buforuje dane z `Products` tabeli, używając na podstawie czasu wygaśnięcia 60 sekund. Pozwól s aktualizacji tej klasy, tak aby używał zależności pamięci podręcznej SQL zamiast tego. `ProductsCL` Klasy s `AddCacheItem` metody, która jest odpowiedzialny za dodawanie danych do pamięci podręcznej, obecnie zawiera następujący kod:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Zaktualizuj kod w celu użycia `SqlCacheDependency` zamiast obiektu `MasterCacheKeyArray` zależności w pamięci podręcznej:


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Aby przetestować tę funkcję, należy dodać GridView do strony poniżej istniejącego `ProductsDeclarative` GridView. Ustaw ten nowy s GridView `ID` do `ProductsProgrammatic` i za pośrednictwem tagu inteligentnego powiązać go do nowego elementu ObjectDataSource, o nazwie `ProductsDataSourceProgrammatic`. Konfigurowanie kontrolki ObjectDataSource używać `ProductsCL` klasy, listy rozwijane w polu Wybierz i aktualizacji karty, aby `GetProducts` i `UpdateProduct`, odpowiednio.


[![Konfigurowanie kontrolki ObjectDataSource na korzystanie z klasy ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Rysunek 11**: Konfigurowanie kontrolki ObjectDataSource do użycia `ProductsCL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image16.png))


[![Wybierz metodę GetProducts z listy rozwijanej wybierz kartę s](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Rysunek 12**: Wybierz `GetProducts` metodę z listy rozwijanej s Wybierz kartę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image18.png))


[![Wybierz metodę UpdateProduct z listy rozwijanej s kartę aktualizacji](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Rysunek 13**: Wybierz metodę UpdateProduct s kartę aktualizacji listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-vb/_static/image20.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio utworzy BoundFields i CheckBoxFields w widoku GridView dla każdego pola danych. Tak, jak przy użyciu GridView pierwszy dodane do tej strony, Usuń wszystkie pola, ale `ProductName`, `CategoryName`, i `UnitPrice`i sformatuj te pola, zgodnie z potrzebami. Za pomocą tagu inteligentnego s GridView zaznacz pola wyboru włączone stronicowanie, włączyć sortowanie i Włącz edytowanie. Podobnie jak w przypadku `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio ustawi `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` właściwość `original_{0}`. Funkcja Edytuj s GridView działało poprawnie, ustaw tę właściwość z powrotem do `{0}` (lub całkowicie usunąć przypisania właściwości z składni deklaratywnej).

Po zakończeniu tych zadań, wynikowy kontrolkami GridView i kontrolki ObjectDataSource oznaczeniu deklaracyjnym powinien wyglądać następująco:


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Aby przetestować SQL zależności pamięci podręcznej w warstwie buforowania Ustaw punkt przerwania `ProductCL` klasy s `AddCacheItem` metody i następnie debugowania rozpoczęcia. Podczas pierwszej wizyty w witrynie `SqlCacheDependencies.aspx`, trafiony punkt przerwania, ponieważ dane są żądane po raz pierwszy i umieszczane w pamięci podręcznej. Następnie przenieść do innej strony w widoku GridView lub jedna z kolumn sortowania. Powoduje to, że GridView zostać ponowiona swoje dane, ale dane powinny znajdować się w pamięci podręcznej od momentu `Products` tabeli bazy danych nie zostały zmodyfikowane. Jeśli dane wielokrotnie nie zostanie znaleziony w pamięci podręcznej, upewnij się, że na komputerze jest dostępna wystarczająca ilość pamięci i spróbuj ponownie.

Po stronicować na kilku stronach GridView, Otwórz drugie okno przeglądarki i przejdź do samouczka podstawy w edytowanie, wstawianie i usuwanie sekcji (`~/EditInsertDelete/Basics.aspx`). Zaktualizuj rekord z tabeli Produkty i następnie w pierwszym oknie przeglądarki, Wyświetl nowej strony lub kliknij jeden z nagłówków sortowania.

W tym scenariuszu zostanie wyświetlony jeden z dwóch kwestii: albo punkt przerwania zostanie uruchomiona, wskazujący, że dane w pamięci podręcznej została wykluczona z powodu zmiany w bazie danych. lub, punkt przerwania nie są osiągane, co oznacza, że `SqlCacheDependencies.aspx` pokazywany nieaktualnych danych. Jeśli nie zostanie osiągnięty punkt przerwania, prawdopodobnie ponieważ usługi sondowania nie zostało jeszcze uruchomione od zmiany danych. Należy pamiętać, że usługa sondowania sprawdza zmiany `Products` tabeli co `pollTime` milisekund, więc istnieje opóźnienie między po zaktualizowaniu danych bazowych i kiedy zostanie wykluczony dane w pamięci podręcznej.

> [!NOTE]
> To opóźnienie jest bardziej prawdopodobne, które będą wyświetlane po edycji produktów dzięki GridView w `SqlCacheDependencies.aspx`. W [buforowania danych w architekturze](caching-data-in-the-architecture-vb.md) samouczek dodaliśmy `MasterCacheKeyArray` pamięci podręcznej zależności, aby potwierdzić, że dane edytowany przy użyciu `ProductsCL` klasy s `UpdateProduct` metoda została wykluczona z pamięci podręcznej. Jednak zastąpiliśmy tej zależności pamięci podręcznej podczas modyfikowania `AddCacheItem` metoda wcześniej w tym kroku i w związku z tym `ProductsCL` klasy będą w dalszym ciągu Pokaż dane w pamięci podręcznej do momentu zmiany — informacje o systemie sondowania `Products` tabeli. Zobaczymy, jak ponownie wprowadzić `MasterCacheKeyArray` zależności w kroku 7 w pamięci podręcznej.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Krok 7: Skojarzenie wiele zależności z elementu w pamięci podręcznej

Pamiętamy `MasterCacheKeyArray` zależności pamięci podręcznej służy do upewnij się, że *wszystkich* dane dotyczące produktu zostanie usunięty z pamięci podręcznej po zaktualizowaniu dowolnego pojedynczego elementu skojarzone znajdujący się w nim. Na przykład `GetProductsByCategoryID(categoryID)` pamięci podręczne metody `ProductsDataTables` wystąpień dla każdego unikatowy *categoryID* wartość. Jeśli jeden z tych obiektów zostanie usunięty, `MasterCacheKeyArray` zależności pamięci podręcznej zapewnia także inne usunięte. Bez tej zależności pamięci podręcznej po zmodyfikowaniu dane w pamięci podręcznej istnieje możliwość, że inne dane w pamięci podręcznej produktu mogą być nieaktualne. W związku z tym, jego s pamiętać, że firma Microsoft zachowuje `MasterCacheKeyArray` przechowują w pamięci podręcznej zależności używanie zależności pamięci podręcznej SQL. Jednak dane w pamięci podręcznej s `Insert` metoda zezwala tylko dla obiektu jednej zależności.

Ponadto podczas pracy z zależności pamięci podręcznej SQL firma Microsoft może być konieczne do skojarzenia z wielu tabel bazy danych jako zależności. Na przykład `ProductsDataTable` pamięci podręcznej w `ProductsCL` klasa zawiera nazwy kategorii i dostawcy dla każdego produktu, ale `AddCacheItem` metoda wykorzystuje tylko zależności w `Products`. W takiej sytuacji, jeśli użytkownik aktualizuje nazwę kategorii lub dostawcy, produktu pamięci podręcznej danych zostanie pozostają w pamięci podręcznej i być nieaktualne. W związku z tym, chcemy dane buforowane produktu zależy nie tylko `Products` tabeli, ale na `Categories` i `Suppliers` także tabele.

[ `AggregateCacheDependency` Klasy](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) umożliwia kojarzenie wielu zależności z elementu pamięci podręcznej. Rozpocznij od utworzenia `AggregateCacheDependency` wystąpienia. Następnie dodaj zestaw zależności za pomocą `AggregateCacheDependency` s `Add` metody. Podczas wstawiania elementu do pamięci podręcznej danych po tej dacie, Przekaż `AggregateCacheDependency` wystąpienia. Gdy *wszelkie* z `AggregateCacheDependency` wystąpienia s zależności zmienią się, zostanie wykluczona element pamięci podręcznej.

Poniżej pokazano zaktualizowany kod dla `ProductsCL` klasy s `AddCacheItem` metody. Ta metoda tworzy `MasterCacheKeyArray` pamięci podręcznej zależności wraz z `SqlCacheDependency` obiektów dla `Products`, `Categories`, i `Suppliers` tabel. Te są wszystkie połączone w jedną `AggregateCacheDependency` obiektu o nazwie `aggregateDependencies`, który jest następnie przekazywany do `Insert` metody.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Testowanie nowego kodu out. Teraz zmienia się na `Products`, `Categories`, lub `Suppliers` tabele spowodować, że dane w pamięci podręcznej zostać wykluczony. Ponadto `ProductsCL` klasy s `UpdateProduct` metody, która jest wywoływana podczas edytowania produktu za pośrednictwem widoku GridView, wyklucza mogą `MasterCacheKeyArray` zależności, co powoduje, że buforowane w pamięci podręcznej `ProductsDataTable` wykluczenie i ponowne pobranie na następnej danych żądanie.

> [!NOTE]
> Zależności pamięci podręcznej SQL może również służyć za pomocą [buforowania danych wyjściowych](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Do pokazania tej funkcji, zobacz: [przy użyciu buforowania danych wyjściowych ASP.NET z programem SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Podsumowanie

Podczas buforowania danych bazy danych, dopóki nie zostanie zmodyfikowany w bazie danych najlepiej pozostanie w pamięci podręcznej. Za pomocą programu ASP.NET 2.0 można tworzyć i używane w scenariuszach zarówno deklaracyjne i programowe zależności pamięci podręcznej SQL. Jednym z wyzwań w tym podejściu Trwa odnajdywanie, gdy dane zostały zmodyfikowane. Pełne wersje programu Microsoft SQL Server 2005 oferują możliwości powiadomień, które może generować alerty aplikacji, gdy wynik zapytania został zmieniony. Express Edition programu SQL Server 2005 i starsze wersje programu SQL Server system sondowania należy użyć zamiast tego. Na szczęście konfigurowania infrastruktury wymaganych sondowania jest dość prosta.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Przy użyciu powiadomienia o zapytaniach w programie Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Tworzenie powiadomień o zapytaniach](https://msdn.microsoft.com/library/ms188669.aspx)
- [Buforowanie w technologii ASP.NET za pomocą `SqlCacheDependency` klasy](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Narzędzie rejestracji serwera SQL platformy ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Omówienie `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Marko Rangel Teresa Murphy i Hilton Giesenow. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-at-application-startup-vb.md)
