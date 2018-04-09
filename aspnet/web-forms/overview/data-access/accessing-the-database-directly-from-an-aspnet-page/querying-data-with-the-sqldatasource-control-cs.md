---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Wykonywanie zapytania na danych z formantem SqlDataSource (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednim samouczki użyliśmy kontrolki ObjectDataSource pełni Rozdziel warstwę prezentacji z warstwy dostępu do danych. Począwszy od tej instruktora...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d6f681169267ad5c65486c1d1fac0a9396535d1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Wykonywanie zapytania na danych z formantem SqlDataSource (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) lub [pobierania plików PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> W poprzednim samouczki użyliśmy kontrolki ObjectDataSource pełni Rozdziel warstwę prezentacji z warstwy dostępu do danych. Począwszy od tego samouczka, możemy Dowiedz się, jak kontrolka SqlDataSource może służyć do prostych aplikacji, które nie wymagają strict separacji prezentacji i dostęp do danych.


## <a name="introduction"></a>Wprowadzenie

Wszystkie samouczków możemy stawienia zbadać wykonanej do tej pory zostały użyte to architektura warstwowe składające się z prezentacji, logiki biznesowej i warstwy dostępu do danych. Warstwa dostępu do danych (DAL) został, co w pierwszym samouczku ([tworzenie Warstwa dostępu do danych](../introduction/creating-a-data-access-layer-cs.md)) i warstwy logiki biznesowej w ciągu sekundy ([Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-cs.md)). Począwszy od [wyświetlanie danych z ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) samouczka widzieliśmy sposób użycia nowej kontrolki ObjectDataSource ASP.NET 2.0 s deklaratywnie interfejsu z architekturą z warstwy prezentacji.

Podczas wszystkich samouczków wykonanej do tej pory używano architektury do pracy z danymi, istnieje również możliwość dostępu, wstawiania, aktualizowania i usuwania bazy danych bezpośrednio ze strony programu ASP.NET, pomijanie architektury. Dzięki temu umieszczenie zapytań określonej bazy danych i logiki biznesowej bezpośrednio na stronie sieci web. Wystarczająco dużych lub złożonych aplikacji projektowanie, wdrażanie i przy użyciu architektury warstwowych ma zasadnicze znaczenie dla sukcesu, aktualizacji i łatwości konserwacji aplikacji. Tworzenie niezawodna architektura, jednak może być konieczne podczas tworzenia niezwykle proste, jednorazowe aplikacji.

ASP.NET 2.0 zapewnia pięć wbudowanych danych źródłowych kontrolek [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), i [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource może służyć do dostępu i modyfikowania dane bezpośrednio z relacyjnej bazy danych, w tym programu Microsoft SQL Server, programu Microsoft Access, Oracle, MySQL i inne. W tym samouczku i dalej trzech zajmiemy się, jak pracować z formantem SqlDataSource, eksploracji, jak wykonać zapytanie i filtrowanie danych bazy danych, a także sposób użycia SqlDataSource do wstawiania, aktualizowania i usuwania danych.


![ASP.NET 2.0 Includes Five Built-In Data Source Controls](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Rysunek 1**: program ASP.NET 2.0 zawiera pięć kontrolki źródła danych wbudowane


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Porównanie SqlDataSource i ObjectDataSource

Koncepcyjnie kontrolki ObjectDataSource i SqlDataSource są po prostu serwery proxy danych. Zgodnie z opisem w [wyświetlanie danych z ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) samouczka ObjectDataSource ma właściwości, które wskazują typ obiektu, który zawiera dane i metody do wywołania, aby wybrać, wstawiania, aktualizowania i usuwania danych z podstawowego typu obiektu. Po skonfigurowaniu właściwości s ObjectDataSource danych formantu sieci Web, takich jak GridView, widoku DetailsView lub DataList może być powiązana z formantu przy użyciu ObjectDataSource s `Select()`, `Insert()`, `Delete()`, i `Update()` metody interakcje z podstawową architekturę.

SqlDataSource zapewnia te same funkcje, ale działa względem relacyjnej bazy danych, a nie biblioteką obiektów. SqlDataSource firma Microsoft, należy określić parametry połączenia bazy danych i zapytania SQL ad hoc lub przechowywanych procedur w celu wykonania do wstawiania, aktualizacji, usuwanie i pobierania danych. SqlDataSource s `Select()`, `Insert()`, `Update()`, i `Delete()` metod, gdy została wywołana, połączenia z określoną bazą danych i odpowiednie zapytanie SQL. Jak na poniższym diagramie przedstawiono, te metody wykonywania pracy grunt, połączenia z bazą danych, zapytania i zwraca wyniki.


![SqlDataSource służy jako serwer Proxy do bazy danych](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Rysunek 2**: SqlDataSource służy jako serwer Proxy do bazy danych


> [!NOTE]
> W tym samouczku firma Microsoft będzie skupić się na pobieranie danych z bazy danych. W [Wstawianie, aktualizowanie i usuwania danych z formantem SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) samouczka zajmiemy się tym, jak skonfigurować SqlDataSource do obsługi Wstawianie, aktualizowanie i usuwanie.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource i AccessDataSource formantów

Oprócz kontroli SqlDataSource ASP.NET 2.0 obejmuje również kontrolkę AccessDataSource. Te dwie inne formanty prowadzić wielu deweloperów jesteś nowym użytkownikiem programu ASP.NET 2.0 podejrzewać, że kontrolka AccessDataSource jest przeznaczona do pracy wyłącznie z programu Microsoft Access za pomocą formantu SqlDataSource zaprojektowane do pracy wyłącznie z programu Microsoft SQL Server. Gdy w elemencie AccessDataSource jest przeznaczona do pracy z programu Microsoft Access, kontroli SqlDataSource współpracuje z *żadnych* relacyjnej bazy danych, który jest możliwy za pośrednictwem platformy .NET. W tym wszelkie OLE DB lub ODBC-CLS magazyny danych, takich jak Microsoft SQL Server, programu Microsoft Access, Oracle, Informix, MySQL i PostgreSQL, wśród wielu innych.

Wyłącznie różnica między formantami AccessDataSource i SqlDataSource jest, jak określono informacje o połączeniu. Kontrolka AccessDataSource wymaga tylko ścieżka do pliku bazy danych programu Access. SqlDataSource, z drugiej strony, wymaga parametrów połączenia ukończone.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Krok 1: Tworzenie stron sieci Web w źródle SqlDataSource

Zanim Rozpoczniemy eksploracji sposób pracy bezpośrednio z bazy danych za pomocą kontroli SqlDataSource, chętnie s najpierw Poświęć chwilę, aby utworzyć w naszym projekt witryny sieci Web, które będą potrzebne dla tego samouczka i dalej trzy stron ASP.NET. Rozpocznij od dodania nowy folder o nazwie `SqlDataSource`. Następnie dodaj do tego folderu, upewniając się skojarzyć każdą stronę z następujących stron ASP.NET `Site.master` strony głównej:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Dodawanie stron ASP.NET samouczki dotyczące SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Rysunek 3**: Dodawanie stron ASP.NET samouczki dotyczące SqlDataSource


Podobnie jak w innych folderach `Default.aspx` w `SqlDataSource` folderu spowoduje wyświetlenie listy samouczków w sekcji. Odwołania, który `SectionLevelTutorialListing.ascx` kontrola użytkownika zapewnia tę funkcję. W związku z tym Dodaj formant użytkownika `Default.aspx` przeciągając je z Eksploratora rozwiązań na stronę s widoku Projekt.


[![Dodaj kontrolkę użytkownika SectionLevelTutorialListing.ascx do Default.aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Rysunek 4**: Dodaj `SectionLevelTutorialListing.ascx` formantu użytkownika `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Na koniec Dodaj następujące cztery strony jako wpisów `Web.sitemap` pliku. W szczególności, Dodaj następujący kod po przyciski niestandardowe Dodawanie DataList i elementu powtarzanego `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web samouczki, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczki.


![Mapy witryny zawiera teraz wpisy samouczki SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Rysunek 5**: mapy witryny zawiera teraz wpisy samouczki SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Krok 2: Dodawanie i konfigurowanie sterowania SqlDataSource

Uruchamianie przez otwarcie `Querying.aspx` strony `SqlDataSource` folder i Przełącz do widoku projektu. Przeciągnij formant SqlDataSource z przybornika do projektanta i zestaw jej `ID` do `ProductsDataSource`. Podobnie jak w elemencie ObjectDataSource, SqlDataSource nie generuje żadnego wyniku renderowanych i dlatego jest wyświetlany jako szary prostokąt na powierzchni projektu. Aby skonfigurować SqlDataSource, kliknij łącze Konfigurowanie źródła danych z tagów inteligentnych s SqlDataSource.


![Polecenie skonfigurować łącze źródła danych z tagów inteligentnych s SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Rysunek 6**: polecenie skonfigurować łącze źródła danych z tagów inteligentnych s SqlDataSource


Spowoduje to wyświetlenie kreatora skonfiguruj źródło danych SqlDataSource kontroli s. Podczas działania kreatora s różnią się od kontrolki ObjectDataSource s, jej celem jest taki sam, aby zawierają szczegółowe informacje na temat pobierania, wstawiania, aktualizowania i usuwania danych za pośrednictwem źródła danych. Dla SqlDataSource wymaga określenia podstawowej bazy danych do użycia i udostępnianie ad hoc instrukcji SQL lub procedur składowanych.

Pierwszym krokiem Kreator wyświetli monit nam dla bazy danych. Listy rozwijanej obejmuje tych baz danych w sieci web s aplikacji `App_Data` folderu oraz te, które zostały dodane do węzła połączenia danych w Eksploratorze serwera. Od nas kolejnych już dodany ciąg połączenia dla `NORTHWIND.MDF` bazy danych w `App_Data` folderu do naszej projektu s `Web.config` pliku, z listy rozwijanej zawiera odwołanie do tego ciągu połączenia `NORTHWINDConnectionString`. Wybierz ten element z listy rozwijanej, a następnie kliknij przycisk Dalej.


![Z listy rozwijanej wybierz NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Rysunek 7**: Wybierz `NORTHWINDConnectionString` z listy rozwijanej


Po wybraniu bazy danych, Kreator zapyta, zapytania do zwrócenia danych. Firma Microsoft można określić kolumn w tabeli lub widoku, aby zwrócić lub można wprowadzić niestandardową instrukcję SQL lub określić procedury składowanej. Można przełączać się między ten wybór za pośrednictwem Określ niestandardową instrukcję SQL lub procedur składowanych i określ kolumny z tabeli lub wyświetlanie przycisków radiowych.

> [!NOTE]
> W tym przykładzie pierwsze umożliwiają używanie Określ kolumn z tabeli lub widoku opcji s. Firma Microsoft będzie wróć do kreatora w dalszej części tego samouczka i Eksploruj Określ niestandardową instrukcję SQL lub procedurę składowaną opcji.


Rysunek nr 8 przedstawia konfiguracji ekranu instrukcji Select w przypadku wybrania Określ kolumny z tabeli lub widoku przycisku radiowego. Listy rozwijanej zawiera zestaw tabel i widoków w bazie danych Northwind z wybranej tabeli lub kolumny w widoku s wyświetlane na poniższej liście wyboru. W tym przykładzie zwraca let s `ProductID`, `ProductName`, i `UnitPrice` kolumny z `Products` tabeli. Jak rysunek nr 8 przedstawia po tych wyborów kreator przedstawia wynikowa instrukcja SQL `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Zwróć dane z tabeli Produkty](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Rysunek 8**: Przywróć dane z `Products` tabeli


Po skonfigurowaniu kreatora, aby zwrócić `ProductID`, `ProductName`, i `UnitPrice` kolumny z `Products` tabeli, kliknij przycisk Dalej. Ten ekran końcowy zapewnia możliwość sprawdzenia wyników kwerendy skonfigurowane z poprzedniego kroku. Kliknięcie przycisku Test zapytanie wykonuje skonfigurowanego `SELECT` instrukcji i wyświetla wyniki w siatce.


![Kliknij przycisk zapytanie testu, aby przejrzeć zapytanie SELECT](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Rysunek 9**: kliknij przycisk Testuj zapytania do przeglądu Twojej `SELECT` zapytania


Aby zakończyć pracę kreatora, kliknij przycisk Zakończ.

Jak z elementu ObjectDataSource, Kreator s SqlDataSource jedynie przypisuje wartości do właściwości formantu s, czyli [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) i [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) właściwości. Po zakończeniu pracy kreatora, Twoje SqlDataSource kontroli s deklaratywne znaczników powinien wyglądać podobny do następującego:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

`ConnectionString` Właściwość zawiera informacje na temat połączenia z bazą danych. Tej właściwości można przypisać wartość ciągu pełnej, stałe połączenie lub może wskazywać na parametry połączenia w `Web.config`. Aby odwołać się do wartości ciągu połączenia w pliku Web.config, należy użyć składni `<%$ expressionPrefix:expressionValue %>`. Zazwyczaj *expressionPrefix* jest ConnectionStrings i *expressionValue* jest nazwą ciągu połączenia w `Web.config` [ `<connectionStrings>` sekcji](https://msdn.microsoft.com/library/bf7sd233.aspx). Jednak składnia może służyć do odwołania `<appSettings>` elementy lub zawartość z plików zasobów. Zobacz [omówienie wyrażenia ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) Aby uzyskać więcej informacji na temat tej składni.

`SelectCommand` Właściwość określa ad hoc instrukcji SQL lub procedurę składowaną można wykonać, aby zwrócić dane.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Krok 3: Dodawanie formantu danych sieci Web i powiązania jej z SqlDataSource

Po skonfigurowaniu SqlDataSource może być powiązana z danymi formantu sieci Web, takich jak widok GridView lub widoku DetailsView. W tym samouczku pozwolić, aby wyświetlić dane w widoku GridView s. Z przybornika, przeciągnij element GridView strony, który należy powiązać `ProductsDataSource` SqlDataSource, wybierając źródło danych z listy rozwijanej w tagu s widoku GridView.


[![Dodaj element GridView i powiąż go do kontroli SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Na rysunku nr 10**: Dodaj element GridView i powiąż go do kontroli SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Po wybraniu z listy rozwijanej w tagu s GridView kontroli SqlDataSource był Visual Studio spowoduje automatyczne dodanie elementu BoundField lub CheckBoxField do widoku GridView dla każdej kolumny zwracane przez formant źródła danych. Ponieważ SqlDataSource zwraca trzy kolumny w bazie danych `ProductID`, `ProductName`, i `UnitPrice` istnieją trzy pola w widoku GridView.

Poświęć chwilę, aby skonfigurować s GridView trzech BoundFields. Zmień `ProductName` pola s `HeaderText` dla właściwości Nazwa produktu i `UnitPrice` pola s ceny. Również sformatować `UnitPrice` pole jako walutę. Po wprowadzeniu tych zmian, deklaratywne GridView znaczników w s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Odwiedź stronę tej strony za pośrednictwem przeglądarki. Jak pokazano na rysunku nr 11, widoku GridView wymieniono każdego produktu s `ProductID`, `ProductName`, i `UnitPrice` wartości.


[![Wyświetla widoku GridView każdego produktu s ProductID ProductName i UnitPrice wartości](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Rysunek 11**: s GridView Wyświetla każdego produktu `ProductID`, `ProductName`, i `UnitPrice` wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


W przypadku odwiedzenia strony widoku GridView wywołuje formantu źródła danych s `Select()` metody. Gdy firma Microsoft używa kontrolki ObjectDataSource, to `ProductsBLL` klasy s `GetProducts()` metody. Z SqlDataSource, jednak `Select()` metody nawiąże połączenie z określoną bazą danych i problemy `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, w tym przykładzie). SqlDataSource zwraca jego wyniki, które następnie wylicza widoku GridView, tworzenie wiersza w widoku GridView dla każdego rekordu bazy danych zwrócił.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Funkcje kontroli danych wbudowanych sieci Web i kontroli SqlDataSource

Ogólnie, funkcje wbudowane w danych Web formanty stronicowania, sortowanie, edycję, usuwanie, wstawiania i tak dalej są specyficzne dla formantu danych w sieci Web i nie są zależne od formantu źródła danych, które są używane. Oznacza to wbudowanych stronicowania, sortowanie, edytowanie i usuwanie czy jest on powiązany z ObjectDataSource lub SqlDataSource może korzystać z widoku GridView. Jednak niektóre dane funkcje kontroli sieci Web jest wielkość liter, do kontroli źródła danych, które są używane lub w konfiguracji kontroli s źródła danych.

Na przykład w [wydajnie stronicowania za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) samouczku omówiono, jak to zrobić, domyślnie logikę stronicowania danych sieci Web kontrolki naively zwraca *wszystkie* rekordy z podstawową źródła danych, a następnie wyświetla tylko odpowiednie podzestaw indeks bieżącej strony i liczbie rekordów wyświetlanych na każdej stronie. Ten model jest bardzo mało wydajne, gdy stronicowania za pośrednictwem wystarczająco dużych zestawów wyników. Na szczęście ObjectDataSource można skonfigurować do obsługi stronicowania niestandardowego, która zwraca dokładne podzbiór rekordów do wyświetlenia. SqlDataSource formantu, jednak nie ma właściwości Implementowanie stronicowania niestandardowego.

Inny subtlety stronicowania i sortowania wynika z SqlDataSource. Domyślnie dane zwrócone z elementu SqlDataSource można stronicowanej lub sortowane w widoku GridView. Aby to wykazać, sprawdź opcje włączyć stronicowanie i Włącz sortowania w widoku GridView s tagu w `Querying.aspx` i sprawdź, czy działa zgodnie z oczekiwaniami.

Sortowanie i stronicowanie działa, ponieważ SqlDataSource pobiera danych w bazie danych do typowaniem luźnym zestawu danych. Całkowita liczba rekordów zwróconych przez kwerendę ważnym aspektem do implementowania stronicowania można ustalić z zestawu danych. Ponadto można sortować wyniki s zestawu danych za pośrednictwem widoku danych. Te możliwości są automatycznie używane przez SqlDataSource podczas żądania GridView stronicowanej lub posortowane dane.

SqlDataSource można skonfigurować do zwracania DataReader zamiast zestawu danych, zmieniając jej [ `DataSourceMode` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) z `DataSet` (ustawienie domyślne) do `DataReader`. Może być preferowane przy użyciu elementu DataReader w sytuacjach podczas przekazywania wyników s SqlDataSource do istniejącego kodu, która oczekuje elementu DataReader. Ponadto ponieważ DataReaders obiektów znacznie prostsze niż zestawów danych, oferują lepszą wydajność. Jeśli wprowadzisz tej zmiany, jednak kontroli danych w sieci Web nie można sortować ani strony, ponieważ SqlDataSource nie można ustalić liczbę rekordów są zwracane przez zapytanie, ani nie elementu DataReader oferuje wszystkie techniki sortowanie zwróconych danych.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Krok 4: Używanie instrukcji SQL niestandardowe lub procedury składowanej

Podczas konfigurowania kontroli SqlDataSource, zapytanie zwraca dane można określić w jednym z dwóch metod jako niestandardową instrukcję SQL lub procedurę składowaną lub kolumny z istniejącej tabeli lub widoku. W kroku 2, możemy się zbadana wybranie kolumn z `Products` tabeli. Let s przyjrzeć się przy użyciu niestandardowych instrukcji SQL.

Dodaj inny formant widoku GridView `Querying.aspx` strony i wybrać opcję utworzenia nowego źródła danych z listy rozwijanej w tagu. Następnie wskazać, że dane będą pobrania z bazy danych to spowoduje utworzenie nowej kontrolki SqlDataSource. Nazwa kontrolki `ProductsWithCategoryInfoDataSource`.


![Tworzenie formantu SqlDataSource o nazwie ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Rysunek 12**: Tworzenie formantu SqlDataSource o nazwie `ProductsWithCategoryInfoDataSource`


Następnym ekranie zapyta firmie Microsoft w celu określenia bazy danych. Wybierz jak robiliśmy wstecz na rysunku 7 `NORTHWINDConnectionString` z listy rozwijanej liście i kliknij przycisk Dalej. W konfiguracji ekranu instrukcji Select wybierz Określ niestandardową instrukcję SQL lub procedurę składowaną przycisku radiowego, a następnie kliknij przycisk Dalej. Pojawi się ekran zdefiniować instrukcje niestandardowe lub procedury składowane, który oferuje zakładki SELECT, INSERT, UPDATE i DELETE. Na każdej karcie można wprowadzić niestandardową instrukcję SQL w pole tekstowe lub wybierz procedurę składowaną z listy rozwijanej. W tym samouczku przedstawiono wprowadzanie niestandardową instrukcję SQL; Następny samouczek zawiera przykład, w którym korzysta z procedury składowanej.


![Wprowadź instrukcję SQL niestandardowych lub wybierz procedurę składowaną](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Rysunek 13**: wprowadź instrukcję SQL niestandardowych lub wybierz procedurę składowaną


Niestandardową instrukcję SQL można wprowadzić ręcznie do kontrolki textbox lub być skonstruowany w postaci graficznej przez kliknięcie przycisku konstruktora zapytań. Konstruktor kwerend lub pole tekstowe, użyj następującego zapytania, aby powrócić `ProductID` i `ProductName` pola z `Products` tabeli, używając `JOIN` można pobrać produktu s `CategoryName` z `Categories` tabeli:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![Graficznie tworzenia zapytania za pomocą konstruktora zapytań](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Rysunek 14**: można skonstruować graficznie zapytania przy użyciu konstruktora zapytań


Po określeniu zapytanie, kliknij przycisk Dalej, aby przejść do ekranu zapytania testu. Kliknij przycisk Zakończ, aby zakończyć pracę kreatora SqlDataSource.

Po zakończeniu działania kreatora widoku GridView będzie mieć trzy BoundFields dodane do niego wyświetlanie `ProductID`, `ProductName`, i `CategoryName` kolumny zwróconych przez kwerendę i spowodować, że następujące deklaratywne znaczników:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![Każdy identyfikator produktu s Nazwa kategorii nazwę i skojarzone pokazuje widoku GridView](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Rysunek 15**: identyfikator s GridView przedstawia każdego produktu, nazwa i nazwa kategorii skojarzona ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy jak wykonywać zapytania i wyświetlać dane za pomocą kontroli SqlDataSource. Podobnie jak ObjectDataSource SqlDataSource służy jako serwer proxy, zapewniając deklaratywne podejście do uzyskiwania dostępu do danych. Określ właściwości bazy danych, aby nawiązać połączenie i SQL `SELECT` kwerendy można wykonać; można można określić za pomocą okna właściwości lub za pomocą Kreatora konfigurowania źródła danych.

`SELECT` Przykłady zapytania możemy się zbadana w tym samouczku zwrócone wszystkie rekordy z określonego zapytania. Formant SqlDataSource, jednak mogą obejmować `WHERE` klauzuli z parametrami, których wartości są przypisane programistycznie lub są automatycznie pobierane z określonego źródła. Zajmiemy się sposobu tworzenia i używania sparametryzowanych zapytań w następnym samouczku!

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu do danych relacyjnych baz danych](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Informacje o formancie SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Samouczki platformy ASP.NET — Szybki Start: SqlDataSource formantu](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Pliku Web.config `<connectionStrings>` — Element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Odwołanie do ciągu połączenia bazy danych](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Susan Connery Bernadette Leigh i Suru Dominika. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-parameterized-queries-with-the-sqldatasource-cs.md)
