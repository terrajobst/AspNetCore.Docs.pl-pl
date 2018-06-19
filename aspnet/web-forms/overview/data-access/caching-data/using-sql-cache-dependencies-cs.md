---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: Przy użyciu zależności buforu SQL (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Najprostsza strategii buforowania jest umożliwienie buforowane dane wygaśnie po upływie określonego czasu. Jednak takie podejście proste oznacza, że maintai buforowane dane...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: 8660fd979b5f18ed0182c8ae1e671f362f5dec7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877986"
---
<a name="using-sql-cache-dependencies-c"></a>Przy użyciu zależności buforu SQL (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) lub [pobierania plików PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> Najprostsza strategii buforowania jest umożliwienie buforowane dane wygaśnie po upływie określonego czasu. Jednak takie podejście proste oznacza, że buforowane dane utrzymuje żaden związek z jego źródle danych, stare dane przechowywane jest za długa lub bieżących danych, który wygasł zbyt szybko. Lepszym rozwiązaniem jest użycie klasy SqlCacheDependency tak, aby danych pozostaje w pamięci podręcznej aż do jej odpowiednie dane został zmodyfikowany w bazie danych SQL. W tym samouczku przedstawiono sposób.


## <a name="introduction"></a>Wprowadzenie

Zbadać buforowania techniki [buforowanie danych z elementu ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) i [buforowania danych w architekturze](caching-data-in-the-architecture-cs.md) samouczki umożliwia wykluczenie danych z pamięci podręcznej po określonym na podstawie czasu wygaśnięcia okres. Ta metoda jest najprostszym sposobem, aby równoważyć wzrost wydajności buforowania przed nieaktualności danych. Wybierając czas wygaśnięcia *x* czas w sekundach, deweloper strony concedes korzystać z zalet wydajności buforowanie tylko *x* sekund, ale można rest łatwe czy swoje dane nigdy nie będą starych dłuższy niż maksymalna z *x* sekund. Oczywiście dla danych statycznych *x* może zostać rozszerzony do użytkowania aplikacji sieci web jako zostały sprawdzone w [buforowanie danych przy uruchamianiu aplikacji](caching-data-at-application-startup-cs.md) samouczka.

Podczas buforowania danych w bazie danych, na podstawie czasu wygaśnięcia często jest wybrany dla jego łatwość użycia, ale często jest nieodpowiedni rozwiązania. W idealnym przypadku danych w bazie danych pozostanie buforowanego dopóki danych został zmodyfikowany w bazie danych. następnie będzie można wykluczyć pamięci podręcznej. Takie podejście maksymalizuje buforowanie zwiększenia wydajności i minimalizuje czas trwania starych danych. Jednak aby można było korzystać z tych zalet musi być niektóre systemu w miejscu, które wie, kiedy danych bazy danych została zmodyfikowana i wyklucza mogą odpowiednich elementów z pamięci podręcznej. Przed składnika ASP.NET 2.0 deweloperzy strony zostały odpowiedzialnych za wdrażanie tego systemu.

Program ASP.NET 2.0 zapewnia [ `SqlCacheDependency` klasy](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) i infrastruktury wymaganej do ustalenia, kiedy nastąpiła zmiana w bazie danych, aby odpowiednie elementy w pamięci podręcznej może zostać wykluczony. Istnieją dwie metody ustalania zmiany danych podstawowych: powiadomień i sondowania. Po omówieniu różnice między powiadomień i sondowania, utworzymy infrastruktury niezbędnych do obsługi sondowania i następnie eksplorować sposób użycia `SqlCacheDependency` klasy w deklaratywnej i programowo scenariuszy.

## <a name="understanding-notification-and-polling"></a>Opis powiadomień i sondowania

Istnieją dwie metody, które mogą służyć do określenia, kiedy dane w bazie danych zostały zmodyfikowane: powiadomień i sondowania. Powiadomienie bazy danych automatycznie alerty środowiska uruchomieniowego ASP.NET po wyników określonego zapytania zostały zmienione od czasu ostatniego wykonano zapytanie, w takim przypadku wykluczaniu są buforowane elementy skojarzone z zapytania. Z sondowania, serwer bazy danych przechowuje informacje o podczas ostatniego zostały zaktualizowane określonego tabel. Środowisko uruchomieniowe programu ASP.NET okresowo sonduje bazy danych, aby sprawdzić, jakie tabele zostały zmienione od momentu ich wprowadzenia do pamięci podręcznej. Te tabele, których dane zostały zmodyfikowane ma ich elementów pamięci podręcznej skojarzonych wykluczony.

Opcja powiadomienie wymaga instalacji mniej niż sondowania i jest bardziej szczegółowego, ponieważ śledzi zmiany na poziomie zapytania, a nie na poziomie tabeli. Niestety powiadomienia są dostępne tylko w pełnej wersji programu Microsoft SQL Server 2005 (tj. wersje-Express). Opcja sondowania można jednak dla wszystkich wersji programu Microsoft SQL Server w wersji 7.0 2005. Ponieważ te samouczki używa wersji Express programu SQL Server 2005, możemy koncentruje się na temat instalowania i przy użyciu opcji sondowania. Zapoznaj się w sekcji dalsze odczytu na końcu tego samouczka dla dalszego zasobów na możliwości powiadomień s programu SQL Server 2005.

Z sondowania, bazy danych musi być skonfigurowana do zawierać tabeli o nazwie `AspNet_SqlCacheTablesForChangeNotification` mający trzy kolumny - `tableName`, `notificationCreated`, i `changeId`. Ta tabela zawiera wiersz dla każdej tabeli, która zawiera dane, których konieczna może być używana w zależności bufora SQL, w aplikacji sieci web. `tableName` Kolumna określa nazwę tabeli podczas `notificationCreated` wskazuje datę i godzinę wiersz został dodany do tabeli. `changeId` Kolumny jest typu `int` i ma początkowej wartości 0. Wartość jest zwiększany po każdej modyfikacji do tabeli.

Oprócz `AspNet_SqlCacheTablesForChangeNotification` tabeli bazy danych również musi zawierać Wyzwalacze w każdej z tabel, które mogą występować w zależności bufora SQL. Te wyzwalacze są wykonywane przy każdym wstawiania, aktualizacji lub usuwania wiersza i zwiększyć tabeli s `changeId` wartość w `AspNet_SqlCacheTablesForChangeNotification`.

Środowisko uruchomieniowe programu ASP.NET śledzi bieżącą `changeId` dla tabeli, gdy buforowanie danych przy użyciu `SqlCacheDependency` obiektu. Okresowo zaznaczono bazy danych oraz wszelkie `SqlCacheDependency` obiekty, których `changeId` różni się od wartości w bazie danych są wykluczony, ponieważ różniących się `changeId` wartość oznacza, że wprowadzono zmiany do tabeli od danych był buforowany.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Krok 1: Eksploracji`aspnet_regsql.exe`wiersza polecenia

Z podejścia sondowania bazy danych należy skonfigurować zawiera infrastruktury opisane powyżej: wstępnie zdefiniowanej tabeli (`AspNet_SqlCacheTablesForChangeNotification`), kilka procedur składowanych i wyzwalacze w każdej z tabel, które mogą być używane w zależności buforu SQL w sieci web aplikacja. Te tabele, procedury składowane i wyzwalaczy można tworzyć przy użyciu wiersza polecenia programu `aspnet_regsql.exe`, który znajduje się w `$WINDOWS$\Microsoft.NET\Framework\version` folderu. Aby utworzyć `AspNet_SqlCacheTablesForChangeNotification` tabeli i skojarzone procedur składowanych, uruchom następujące polecenie w wierszu polecenia:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Do wykonania tych poleceń logowania określona baza danych musi być w [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) i [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) ról. Aby sprawdzić T-SQL wysyłane do bazy danych przez `aspnet_regsql.exe` wiersza polecenia, zapoznaj się [ten wpis w blogu](http://scottonwriting.net/sowblog/posts/10709.aspx).


Na przykład, aby dodać infrastruktury do sondowania bazą danych programu Microsoft SQL Server o nazwie `pubs` na serwerze bazy danych o nazwie `ScottsServer` przy użyciu uwierzytelniania systemu Windows, przejdź do odpowiedniego katalogu i, w wierszu polecenia wpisz:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

Po dodaniu bazy danych na poziomie infrastruktury, należy dodać wyzwalaczy tych tabel, które będą używane w zależności buforu SQL. Użyj `aspnet_regsql.exe` wiersza polecenia programu ponownie, ale ją określić za pomocą nazwy tabeli `-t` przełącznika i zamiast `-ed` Użyj przełącznika `-et`, w następujący sposób:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Aby dodać wyzwalaczy do `authors` i `titles` tabel na `pubs` bazy danych na `ScottsServer`, użyj:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

W tym samouczku dodać wyzwalaczy do `Products`, `Categories`, i `Suppliers` tabel. Przyjrzymy składni wiersza polecenia określonego w kroku 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Krok 2: Odwołuje się do programu Microsoft SQL Server 2005 Express Edition bazy danych w`App_Data`

`aspnet_regsql.exe` Wiersza polecenia programu wymaga nazwy bazy danych i serwera, aby można było dodać infrastruktury niezbędnych sondowania. Ale co to jest nazwa bazy danych i serwera dla programu Microsoft SQL Server 2005 Express bazy danych, która znajduje się w `App_Data` folderu? Zamiast odnajdywanie, jakie są nazwy bazy danych i serwera, I Zapisz odnaleźć najprostsza metoda można dołączyć bazy danych do `localhost\SQLExpress` bazy danych, wystąpienia i Zmień nazwę danych przy użyciu [programu SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Jeśli masz pełnej wersji programu SQL Server 2005 na komputerze jest zainstalowany, następnie prawdopodobnie masz już zainstalowany na tym komputerze program SQL Server Management Studio. Jeśli masz tylko wersji Express edition, możesz pobrać bezpłatną [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Rozpocznij od zamknięcia programu Visual Studio. Następnie otwórz program SQL Server Management Studio i wybrać połączenie `localhost\SQLExpress` serwera przy użyciu uwierzytelniania systemu Windows.


![Dołącz do localhost\SQLExpress serwera](using-sql-cache-dependencies-cs/_static/image1.gif)

**Rysunek 1**: dołączanie do `localhost\SQLExpress` serwera


Po nawiązaniu połączenia z serwerem Management Studio Pokaż serwera i podfoldery dla baz danych, zabezpieczeń i tak dalej. Kliknij prawym przyciskiem folder baz danych i wybierz opcję dołączenia. Zostanie wyświetlone okno dialogowe dołączanie bazy danych (zobacz rysunek 2). Kliknij przycisk Dodaj, a następnie wybierz `NORTHWND.MDF` folder bazy danych w sieci web aplikacji s `App_Data` folderu.


[![Dołącz NORTHWND. MDF bazy danych, z folderu App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**Rysunek 2**: Dołącz `NORTHWND.MDF` bazy danych z `App_Data` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image2.png))


Spowoduje to dodanie bazy danych do folderu bazy danych. Nazwa bazy danych może być pełną ścieżkę do pliku lub pełną ścieżkę poprzedzony przez [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Aby uniknąć konieczności wpisz nazwę tego długich bazy danych przy użyciu aspnet\_regsql.exe narzędzia wiersza polecenia, Zmień nazwę bazy danych do nazwy bardziej przyjaznych dla człowieka, klikając prawym przyciskiem myszy w bazie danych tylko dołączona i wybierając pozycję Zmień nazwę. I Zapisz zmieniona bazy danych na DataTutorials.


![Zmień nazwę bazy danych dołączona na nazwę bardziej przyjaznych dla człowieka](using-sql-cache-dependencies-cs/_static/image3.gif)

**Rysunek 3**: Zmień nazwę bazy danych dołączona na nazwę bardziej przyjaznych dla człowieka


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Krok 3: Dodawanie infrastruktury sondowania z bazą danych Northwind

Teraz, gdy będziemy mieć dołączony `NORTHWND.MDF` bazy danych z `App_Data` folderu, możemy re gotowe do dodania infrastruktury sondowania. Przy założeniu, że był nazwy bazy danych do DataTutorials, uruchom następujące polecenia cztery:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

Po uruchomieniu tych poleceń czterech, kliknij prawym przyciskiem myszy nazwę bazy danych w programie Management Studio, przejdź do menu zadania i wybierz odłączeń. Następnie zamknij program Management Studio i otwórz ponownie program Visual Studio.

Po Visual Studio ma ponownie otwarty, przejdź do bazy danych za pomocą Eksploratora serwera. Należy zwrócić uwagę nową tabelę (`AspNet_SqlCacheTablesForChangeNotification`), nowe procedury składowane i wyzwalaczy na `Products`, `Categories`, i `Suppliers` tabel.


![Baza danych zawiera teraz infrastruktury niezbędnych sondowania](using-sql-cache-dependencies-cs/_static/image4.gif)

**Rysunek 4**: baza danych zawiera teraz infrastruktury niezbędnych sondowania


## <a name="step-4-configuring-the-polling-service"></a>Krok 4: Konfigurowanie usługi sondowania

Po utworzeniu niezbędne tabele, wyzwalacze i procedury składowane w bazie danych, ostatnim krokiem jest skonfigurowanie usługi sondowania, który odbywa się za pośrednictwem `Web.config` , określając baz danych do użycia i częstotliwość sondowania (w milisekundach). Następujący kod znaczników sonduje bazy danych Northwind raz na sekundę.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

`name` Wartość w `<add>` elementu (NorthwindDB) kojarzy nazwę zrozumiałą dla użytkownika z określoną bazę danych. Podczas pracy z zależności buforu SQL, musimy odwoływać się do nazwy bazy danych oraz tabeli na podstawie buforowane dane zdefiniowane w tym miejscu. Zajmiemy się tym, jak używać `SqlCacheDependency` klasy programowane kojarzenie zależności buforu SQL z pamięci podręcznej danych w kroku 6.

Po ustanowieniu zależności bufora SQL, system sondowania połączy się z bazami danych zdefiniowanych w `<databases>` elementy co `pollTime` milisekund, a następnie wykonaj `AspNet_SqlCachePollingStoredProcedure` procedury składowanej. Kopię tej procedury składowanej — która została dodana w kroku 3 za pomocą `aspnet_regsql.exe` narzędzie wiersza polecenia — zwraca `tableName` i `changeId` wartości dla każdego rekordu w `AspNet_SqlCacheTablesForChangeNotification`. Nieaktualne zależności buforu SQL jest wykluczony z pamięci podręcznej.

`pollTime` Ustawienie wprowadzenie zależności między wydajnością i nieaktualności danych. Małą `pollTime` wartość zwiększa liczbę żądań do bazy danych, ale więcej szybko wyklucza mogą stare dane z pamięci podręcznej. Większy `pollTime` wartość zmniejsza liczbę żądań bazy danych, ale zwiększa opóźnienie między podczas zmiany danych zaplecza, a jeśli wykluczaniu są elementy pokrewne pamięci podręcznej. Na szczęście żądanie bazy danych jest wykonywany prostej procedury składowanej tego s zwracanie tylko kilka wierszy z tabeli proste, lekkie. Ale eksperymentować z różnymi `pollTime` wartości można znaleźć idealną równowagę między bazy danych nieaktualności dostęp i danych aplikacji. Najmniejsza `pollTime` dozwolona wartość to 500.

> [!NOTE]
> Powyższy przykład zawiera jeden `pollTime` wartość w `<sqlCacheDependency>` elementu, ale Opcjonalnie można określić `pollTime` wartość w `<add>` elementu. Jest to przydatne, jeśli masz wiele baz danych określonych i dostosować częstotliwość sondowania na bazę danych.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Krok 5: Deklaratywnie Praca z zależności buforu SQL

W krokach 1 – 4 analizujemy sposobu konfiguracji infrastruktury niezbędnych bazy danych i konfiguracji systemu sondowania. Z tej infrastruktury w miejscu możemy teraz dodawać elementy do pamięci podręcznej danych z skojarzone zależności bufora SQL przy użyciu technik programowe lub deklaratywne. W tym kroku zajmiemy się, jak deklaratywnie pracować z zależności buforu SQL. W kroku 6 przyjrzymy rozwiązania programowego.

[Buforowanie danych z elementu ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) samouczek przedstawione deklaratywnych buforowania elementu ObjectDataSource. Wystarczy wybrać ustawienie `EnableCaching` właściwości `true` i `CacheDuration` właściwości pewnego interwału czasu, ObjectDataSource automatycznie będą buforowane dane zwrócone z jego obiektu źródłowego dla określonego interwału. Element ObjectDataSource można również użyć co najmniej jeden zależności buforu SQL.

Aby zademonstrować, deklaratywnego przy użyciu zależności buforu SQL, otwórz `SqlCacheDependencies.aspx` strony `Caching` folder i przeciągnij element GridView z przybornika do projektanta. Ustaw GridView s `ID` do `ProductsDeclarative` i z jego tagów inteligentnych, wybierz powiązać nowy element ObjectDataSource o nazwie `ProductsDataSourceDeclarative`.


[![Utwórz nowy element ObjectDataSource o nazwie ProductsDataSourceDeclarative](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Rysunek 5**: Utwórz nowy składnik o nazwie ObjectDataSource `ProductsDataSourceDeclarative` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image4.png))


Element ObjectDataSource umożliwia konfigurowanie `ProductsBLL` klasy i ustawić listy rozwijanej wybierz karcie do `GetProducts()`. Na karcie aktualizacji wybierz `UpdateProduct` przeciążenia z trzech parametrów wejściowych - `productName`, `unitPrice`, i `productID`. Ustawianie list rozwijanych (Brak) na kartach INSERT i DELETE.


[![Użyj przeciążenia UpdateProduct z trzech parametrów wejściowych](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Rysunek 6**: trzy parametry wejściowe za pomocą przeciążenia UpdateProduct ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image6.png))


[![Ustaw listy rozwijanej (Brak) dla INSERT i usuwanie kart](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Rysunek 7**: Ustaw na liście rozwijanej na (Brak) dla Wstawianie i usuwanie kart ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image8.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio utworzy BoundFields i CheckBoxFields w widoku GridView dla każdego pola danych. Usuń wszystkie pola, ale `ProductName`, `CategoryName`, i `UnitPrice`i formatowania tych pól, zgodnie z własnymi potrzebami. W widoku GridView s tagu Sprawdź włączyć stronicowanie, Włącz sortowanie i Włącz edytowanie pól wyboru. Visual Studio ustawi ObjectDataSource s `OldValuesParameterFormatString` właściwości `original_{0}`. Aby funkcja edycji s GridView działało poprawnie, albo ta właściwość całkowicie usunąć z składni deklaratywnej lub ustaw ją z powrotem na wartość domyślną `{0}`.

Na koniec należy dodać formantu etykiety Web powyżej widoku GridView i ustaw jej `ID` właściwości `ODSEvents` i jego `EnableViewState` właściwości `false`. Po wprowadzeniu tych zmian, znaczniki deklaratywne s strony powinien wyglądać podobny do następującego. Uwaga tej I kolejnych wprowadzone liczba estetycznych dostosowań do pól widoku GridView, które nie są konieczne do pokazu funkcjonalności zależności buforu SQL.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Następnie należy utworzyć programu obsługi zdarzeń dla elementu ObjectDataSource s `Selecting` zdarzeń i w jego Dodaj następujący kod:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Odwołania, który ObjectDataSource s `Selecting` zdarzenia generowane tylko wtedy, gdy jest to pobieranie danych z jego obiekt źródłowy. Jeśli element ObjectDataSource uzyskuje dostęp do danych z własnej pamięci podręcznej, to zdarzenie nie jest uruchamiany.

Teraz odwiedź stronę tej strony za pośrednictwem przeglądarki. Ponieważ firma Microsoft Przenieś jeszcze do wdrożenia buforowania, zawsze strony, sortowanie i edycji strony siatki powinien być wyświetlany tekst, zdarzenia Selecting uruchamiany, jak pokazano na rysunku 8.


[![Element ObjectDataSource s zdarzenia Selecting generowane każdej zmianie widoku GridView jest stronicowanej, edytować, lub Sorted](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Rysunek 8**: s ObjectDataSource `Selecting` uruchamiany każdy czas trwania zdarzenia widoku GridView jest stronicowanej, zmieniona lub Sorted ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image10.png))


Jak widzieliśmy w [buforowanie danych z elementu ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) samouczek, ustawienie `EnableCaching` właściwości `true` powoduje, że element ObjectDataSource do buforowania danych w czasie trwania określony przez jego `CacheDuration` właściwości. Ma również element ObjectDataSource [ `SqlCacheDependency` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), która dodaje co najmniej jeden zależności buforu SQL w pamięci podręcznej danych przy użyciu wzorca:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Gdzie *databaseName* to nazwa bazy danych, jak określono w `name` atrybutu `<add>` element `Web.config`, i *tableName* to nazwa tabeli bazy danych. Na przykład, aby utworzyć element ObjectDataSource, który buforuje dane w nieskończoność na podstawie zależności bufora SQL, przed Northwind s `Products` tabeli, należy ustawić element ObjectDataSource s `EnableCaching` właściwości `true` i jego `SqlCacheDependency` właściwości NorthwindDB:Products.

> [!NOTE]
> Można użyć zależności bufora SQL *i* na podstawie czasu wygaśnięcia przez ustawienie `EnableCaching` do `true`, `CacheDuration` przedziału czasu i `SqlCacheDependency` do bazy danych i tabeli nazw. Element ObjectDataSource Wyklucz jego danych po przekroczeniu na podstawie czasu wygaśnięcia lub gdy system sondowania uwagi dotyczące zmiana danych bazy danych, zależnie od sytuacji najpierw.


W widoku GridView `SqlCacheDependencies.aspx` są wyświetlane dane z dwóch tabel - `Products` i `Categories` (produkt s `CategoryName` pola są pobierane za pomocą `JOIN` na `Categories`). W związku z tym chcemy, aby określić zależności buforu SQL dwóch: NorthwindDB:Products; NorthwindDB:Categories.


[![Skonfiguruj ObjectDataSource do obsługi pamięci podręcznej przy użyciu zależności buforu SQL, produktów i kategorie](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Rysunek 9**: Konfigurowanie ObjectDataSource do obsługi pamięci podręcznej przy użyciu zależności buforu SQL na `Products` i `Categories` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image12.png))


Po skonfigurowaniu ObjectDataSource do obsługi pamięci podręcznej, ponownie strony za pośrednictwem przeglądarki. Ponownie zdarzenia Selecting tekst uruchamiany powinny pojawiać się na pierwszej wizyty strony, ale powinien zniknie po stronicowania, sortowanie lub przycisków edycji, lub przycisk Anuluj. Jest to spowodowane po załadowaniu danych w pamięci podręcznej s ObjectDataSource pozostaje do `Products` lub `Categories` tabele są modyfikowane lub dane są aktualizowane za pośrednictwem widoku GridView.

Po stronicowania za pośrednictwem siatki i zapisując braku zdarzenia Selecting uruchamiany tekst, Otwórz nowe okno przeglądarki i przejdź do samouczka podstawy w edytowanie, wstawianie i usuwanie sekcji (`~/EditInsertDelete/Basics.aspx`). Zaktualizuj nazwę lub cenę produktu. Następnie, od pierwszego okna przeglądarki, Wyświetl innej strony danych, sortowania siatki lub kliknij przycisk Edytuj wiersz s. Teraz, zdarzenia Selecting uruchamiany powinien pojawić się ponownie, podstawowej bazy danych, którego dane zostały zmienione (zobacz rysunek 10). Jeśli tekst nie jest wyświetlany, poczekaj chwilę i spróbuj ponownie. Należy pamiętać, że usługa sondowania sprawdza, czy zmiany `Products` tabeli co `pollTime` milisekund, więc nastąpi opóźnienie między po zaktualizowaniu danych i gdy dane pamięci podręcznej zostanie usunięty.


[![Modyfikowanie tabeli Produkty wyklucza mogą dane buforowane produktu](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Na rysunku nr 10**: modyfikacja tabeli Produkty wyklucza mogą danych produktu w pamięci podręcznej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Krok 6: Programowane Praca z`SqlCacheDependency`— klasa

[Buforowania danych w architekturze](caching-data-in-the-architecture-cs.md) samouczek przeglądał korzyści wynikające ze stosowania osobnej warstwie buforowanie w architekturze przeciwieństwie ściśle sprzężenia buforowania z ObjectDataSource. W tym samouczku utworzyliśmy `ProductsCL` klasy, aby zademonstrować programowo Praca z pamięci podręcznej danych. Korzystanie z zależności buforu SQL w warstwie buforowania, użyj `SqlCacheDependency` klasy.

W systemie sondowania `SqlCacheDependency` obiekt musi być skojarzone z określonym parą bazy danych i tabeli. Następujący kod, na przykład tworzy `SqlCacheDependency` obiekt na podstawie bazy danych Northwind s `Products` tabeli:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Wprowadź dwa parametry `SqlCacheDependency` Konstruktor s są odpowiednio nazwy bazy danych i tabeli. Jak ObjectDataSource s `SqlCacheDependency` właściwości, nazwa bazy danych używana jest taka sama jak wartość określoną w `name` atrybutu `<add>` element `Web.config`. Nazwa tabeli jest rzeczywistą nazwą tabeli bazy danych.

Aby skojarzyć `SqlCacheDependency` z elementem dodanych do pamięci podręcznej danych, użyj jednej z `Insert` przeciążenia metody, które akceptuje zależności. Poniższy kod dodaje *wartość* do pamięci podręcznej danych na czas nieokreślony, lecz kojarzy ją z `SqlCacheDependency` na `Products` tabeli. Krótko mówiąc *wartość* zostanie umieszczony w pamięci podręcznej, dopóki nie zostanie usunięty z powodu ograniczeń pamięci lub ponieważ sondowania system wykrył, że `Products` tabeli zmieniła się od go w pamięci podręcznej.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

S buforowanie warstwy `ProductsCL` klasy obecnie buforuje dane z `Products` tabeli, używając na podstawie czasu wygaśnięcia 60 sekund. Let s aktualizacji tej klasy, tak aby były używane zamiast zależności buforu SQL. `ProductsCL` Klasy s `AddCacheItem` metodę, która jest odpowiedzialny za dodawanie danych do pamięci podręcznej, w obecnie zawiera następujący kod:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Ten kod, aby używać aktualizacji `SqlCacheDependency` obiekt zamiast `MasterCacheKeyArray` pamięci podręcznej zależności:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Aby przetestować tę funkcję, należy dodać element GridView do strony poniżej istniejące `ProductsDeclarative` widoku GridView. Ustaw to nowe s GridView `ID` do `ProductsProgrammatic` i za pośrednictwem jego tagów inteligentnych powiązać go z nowego elementu ObjectDataSource o nazwie `ProductsDataSourceProgrammatic`. Element ObjectDataSource umożliwia konfigurowanie `ProductsCL` klasy ustawienie z listy rozwijanej wybierz w i aktualizacji karty, aby `GetProducts` i `UpdateProduct`odpowiednio.


[![Skonfiguruj ObjectDataSource do użycia klasy ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Rysunek 11**: Konfigurowanie ObjectDataSource użyć `ProductsCL` klasy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image16.png))


[![Wybierz metodę GetProducts z listy rozwijanej wybierz kartę s](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Rysunek 12**: Wybierz `GetProducts` metodę z listy rozwijanej s Wybierz kartę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image18.png))


[![Wybierz metodę UpdateProduct z listy rozwijanej s kartę aktualizacji](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Rysunek 13**: z listy rozwijanej s kartę aktualizacji należy wybrać metodę UpdateProduct ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-sql-cache-dependencies-cs/_static/image20.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio utworzy BoundFields i CheckBoxFields w widoku GridView dla każdego pola danych. Jak z pierwszego widoku GridView, dodane do tej strony, Usuń wszystkie pola, ale `ProductName`, `CategoryName`, i `UnitPrice`i formatowania tych pól, zgodnie z własnymi potrzebami. W widoku GridView s tagu Sprawdź włączyć stronicowanie, Włącz sortowanie i Włącz edytowanie pól wyboru. Jak `ProductsDataSourceDeclarative` ustawi ObjectDataSource, Visual Studio `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` właściwości `original_{0}`. Funkcja Edytuj s GridView działało poprawnie, ustaw tę właściwość z powrotem do `{0}` (lub całkowicie usunąć przypisanie właściwości z składni deklaratywnej).

Po zakończeniu tych zadań, wynikowy GridView i ObjectDataSource znaczników deklaratywne powinien wyglądać następująco:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

Aby przetestować SQL zależności w pamięci podręcznej w warstwie buforowanie Ustaw punkt przerwania `ProductCL` klasy s `AddCacheItem` — metoda i następnie rozpoczęcia debugowania. Podczas pierwszej wizyty w witrynie `SqlCacheDependencies.aspx`, punkt przerwania powinien trafiony żądano po raz pierwszy i umieszczane w pamięci podręcznej danych. Następnie przenieść do innej strony w widoku GridView lub jednej z kolumn sortowania. Powoduje to GridView do requery swoje dane, ale dane powinny znajdować się w pamięci podręcznej od momentu `Products` tabeli bazy danych nie został zmodyfikowany. Jeśli dane wielokrotnie nie zostanie znaleziony w pamięci podręcznej, upewnij się, że na komputerze jest dostępna wystarczająca ilość pamięci i spróbuj ponownie.

Po stronicowania przez kilka strony GridView, Otwórz drugie okno przeglądarki i przejdź do samouczka podstawy w edytowanie, wstawianie i usuwanie sekcji (`~/EditInsertDelete/Basics.aspx`). Zaktualizuj rekord na podstawie tabeli Produkty i następnie w pierwszym oknie przeglądarki wyświetlić nowej strony lub kliknij jeden z nagłówków sortowania.

W tym scenariuszu zobaczysz jednego z następujących operacji: albo punkt przerwania zostanie uruchomiona, wskazujący, że dane pamięci podręcznej został wykluczony z powodu zmiany w bazie danych. lub, punkt przerwania nie są osiągane, co oznacza, że `SqlCacheDependencies.aspx` jest teraz pokazywanie starych danych. Jeśli nie zostaje trafiony punkt przerwania, prawdopodobnie ponieważ sondowanie usługi nie zostało jeszcze uruchomione od zmiany danych. Należy pamiętać, że usługa sondowania sprawdza, czy zmiany `Products` tabeli co `pollTime` milisekund, więc nastąpi opóźnienie między po zaktualizowaniu danych i gdy dane pamięci podręcznej zostanie usunięty.

> [!NOTE]
> To opóźnienie jest bardziej prawdopodobne są wyświetlane podczas edytowania jeden z nich za pośrednictwem widoku GridView w `SqlCacheDependencies.aspx`. W [buforowania danych w architekturze](caching-data-in-the-architecture-cs.md) samouczek dodaliśmy `MasterCacheKeyArray` pamięci podręcznej zależności do zapewnienia, że dane edytowana za pomocą `ProductsCL` klasy s `UpdateProduct` metody został usunięty z pamięci podręcznej. Jednak ta zależność pamięci podręcznej możemy zastąpione podczas modyfikowania `AddCacheItem` metody wcześniej w tym kroku i w związku z tym `ProductsCL` klasy będą w dalszym ciągu Pokaż dane pamięci podręcznej, dopóki system sondowania uwagi dotyczące zmiany `Products` tabeli. Zajmiemy się tym, jak wprowadzić `MasterCacheKeyArray` pamięci podręcznej zależności w kroku 7.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Krok 7: Kojarzenie wiele zależności z elementu pamięci podręcznej

Odwołania, który `MasterCacheKeyArray` zależności bufora służy do zapewnienia, że *wszystkie* danych dotyczących produktu zostanie usunięty z pamięci podręcznej po zaktualizowaniu dowolny element skojarzony w niej. Na przykład `GetProductsByCategoryID(categoryID)` — metoda pamięci podręcznych `ProductsDataTables` wystąpień dla każdego unikatowy *categoryID* wartość. Jeśli jeden z tych obiektów zostanie usunięty, `MasterCacheKeyArray` zależności bufora zapewnia także inne usunięte. Bez tej zależności bufora modyfikacji buforowane dane istnieje możliwość, że inne dane buforowane produktu mogą być nieaktualne. W rezultacie jego s ważne, firma Microsoft zachowuje `MasterCacheKeyArray` zależności pamięci podręcznej, korzystając z zależności buforu SQL. Jednak dane pamięci podręcznej s `Insert` metody zezwala tylko dla obiekt jednej zależności.

Ponadto podczas pracy z zależności buforu SQL może należy skojarzyć wiele tabel bazy danych jako zależności. Na przykład `ProductsDataTable` pamięci podręcznej w `ProductsCL` klasa zawiera nazwy kategorii i dostawcy dla każdego produktu, ale `AddCacheItem` metoda używa tylko zależności w `Products`. W takiej sytuacji jeśli użytkownik aktualizuje nazwę kategorii lub dostawcy, dane buforowane produktu będzie pozostawać w pamięci podręcznej i być nieaktualne. W związku z tym chcemy udostępnić dane buforowane produktu zależy nie tylko `Products` tabeli, ale na `Categories` i `Suppliers` również tabel.

[ `AggregateCacheDependency` Klasy](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) umożliwia kojarzenie wiele zależności z elementu pamięci podręcznej. Rozpocznij od utworzenia `AggregateCacheDependency` wystąpienia. Następnie dodaj zestaw zależności za pomocą `AggregateCacheDependency` s `Add` metody. Po późniejszym Wstawianie elementu do pamięci podręcznej danych, Przekaż `AggregateCacheDependency` wystąpienia. Gdy *żadnych* z `AggregateCacheDependency` zmienić zależności wystąpienia s, element pamięci podręcznej zostanie usunięty.

Poniżej pokazano zaktualizowany kod `ProductsCL` klasy s `AddCacheItem` metody. Ta metoda tworzy `MasterCacheKeyArray` pamięci podręcznej zależności wraz z `SqlCacheDependency` obiektów na `Products`, `Categories`, i `Suppliers` tabel. Te są wszystkie połączone w jedną `AggregateCacheDependency` obiektu o nazwie `aggregateDependencies`, które są następnie przekazywane do `Insert` metody.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Testowania nowego kodu wychodzących. Teraz zmienia się na `Products`, `Categories`, lub `Suppliers` tabel spowodować wykluczenie w pamięci podręcznej danych. Ponadto `ProductsCL` klasy s `UpdateProduct` metodę, która jest wywoływana podczas edytowania produktu za pośrednictwem widoku GridView, wyklucza mogą `MasterCacheKeyArray` pamięci podręcznej zależności, co powoduje, że zapisane w pamięci podręcznej `ProductsDataTable` wykluczenie i ponowne pobranie przy następnym danych żądanie.

> [!NOTE]
> Zależności buforu SQL można również używać razem [buforowanie danych wyjściowych](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Aby demonstracyjne tej funkcji, zobacz: [przy użyciu buforowanie danych wyjściowych programu ASP.NET z programem SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Podsumowanie

Podczas buforowania danych w bazie danych, dopóki nie zostanie zmodyfikowany w bazie danych w idealnym przypadku pozostanie w pamięci podręcznej. Z programu ASP.NET 2.0 można tworzyć i używane w scenariuszach zarówno deklaratywne i programowe zależności buforu SQL. Jest jednym z wyzwań związanych z tą metodą odnajdywania, gdy dane zostały zmodyfikowane. Pełne wersje programu Microsoft SQL Server 2005 zapewniają możliwości powiadomienie, które może generować alerty aplikacji, gdy zostanie zmieniony w wyniku zapytania. Express Edition programu SQL Server 2005 i starszych wersjach programu SQL Server system sondowania należy zamiast tego użyć. Na szczęście konfigurowania infrastruktury sondowania niezbędne jest bardzo prosta.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Za pomocą powiadomień o zapytaniach w programie Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Tworzenie powiadomienia kwerendy](https://msdn.microsoft.com/library/ms188669.aspx)
- [Buforowanie w programie ASP.NET z `SqlCacheDependency` — klasa](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Narzędzie rejestracji programu ASP.NET SQL Server (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Omówienie `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Marko Rangel Teresa Murphy i Hilton Giesenow. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-at-application-startup-cs.md)
> [dalej](caching-data-with-the-objectdatasource-vb.md)
