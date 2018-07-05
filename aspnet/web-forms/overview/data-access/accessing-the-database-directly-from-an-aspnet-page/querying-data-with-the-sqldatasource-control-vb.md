---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: Wykonywanie zapytania o dane przy użyciu kontrolki SqlDataSource (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednich samouczkach użyliśmy kontrolka ObjectDataSource, aby w pełni rozdzielić warstwy prezentacji z warstwy dostępu do danych. Począwszy od tej instruktora...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d7ec182325d609877ac0d3603a5124b5fbe5a229
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365714"
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>Wykonywanie zapytania o dane przy użyciu kontrolki SqlDataSource (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) lub [Pobierz plik PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> W poprzednich samouczkach użyliśmy kontrolka ObjectDataSource, aby w pełni rozdzielić warstwy prezentacji z warstwy dostępu do danych. Począwszy od tego samouczka dowie się, jak używać kontrolki SqlDataSource dla prostych aplikacji, które nie wymagają takich ścisłą prezentacji i dostęp do danych.


## <a name="introduction"></a>Wprowadzenie

Wszystkie z samouczków firma ve badania do tej pory były używane architektury warstwowej, składający się z prezentacji, logiki biznesowej i warstwy dostępu do danych. Warstwa dostępu do danych (DAL) została specjalnie w pierwszym samouczku ([Tworzenie warstwy dostępu do danych](../introduction/creating-a-data-access-layer-vb.md)) i warstwę logiki biznesowej w ciągu sekundy ([Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md)). Począwszy od [wyświetlanie danych za pomocą kontrolki ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) samouczek widzieliśmy, jak za pomocą nowego formantu ObjectDataSource s ASP.NET 2.0 deklaratywne interfejsu dzięki architekturze z warstwy prezentacji.

Chociaż wszystkich samouczków do tej pory architektury używanych do pracy z danymi, jest również możliwość dostępu, wstawiania, aktualizowania i usuwania bazy danych bezpośrednio ze strony programu ASP.NET, z pominięciem architektury. Ten sposób umieszcza konkretnej bazy danych, zapytań i logikę biznesową bezpośrednio na stronie sieci web. W przypadku aplikacji wystarczająco duży lub złożony zasadnicze znaczenie dla sukcesu, aktualizacji i konserwacji aplikacji jest projektowania, wdrażania i przy użyciu architektury warstwowej. Tworzenie niezawodnych architektury, jednak może być niepotrzebne podczas tworzenia niezwykle proste, jednorazowe aplikacji.

ASP.NET 2.0 zapewnia pięć wbudowanych danych źródłowych kontrolek [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [elementu XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), i [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource może służyć do dostępu i modyfikowania danych bezpośrednio z relacyjnej bazy danych, w tym programu Microsoft SQL Server, Microsoft Access, Oracle, MySQL i inne. W tym samouczku i dalej trzech zajmiemy się, jak działa przy użyciu kontrolki SqlDataSource, eksplorowanie, jak wykonywać zapytania i filtrowanie danych bazy danych, a także sposób korzystania z kontrolką SqlDataSource do Wstawianie, aktualizowanie i usuwanie danych.


![Platforma ASP.NET 2.0 obejmuje pięć formantów źródła danych wbudowane](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Rysunek 1**: Platforma ASP.NET 2.0 obejmuje pięć formantów źródła danych wbudowane


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Porównanie ObjectDataSource i użyciu kontrolki SqlDataSource

Model kontrolki ObjectDataSource i użyciu kontrolki SqlDataSource są po prostu serwery proxy usługi danych. Zgodnie z opisem w [wyświetlanie danych za pomocą kontrolki ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) samouczku kontrolki ObjectDataSource ma właściwości, które wskazuje typ obiektu, który zawiera dane i metody do wywołania, aby wybrać, wstawianie, aktualizowanie i usuwanie danych w typie podstawowym obiektu. Po skonfigurowaniu właściwości s ObjectDataSource danych formantu sieci Web, takich jak GridView, DetailsView lub DataList może być powiązana z formantu za pomocą ObjectDataSource s `Select()`, `Insert()`, `Delete()`, i `Update()` metody Korzystaj z podstawową architekturę.

SqlDataSource udostępnia taką samą funkcjonalność, ale operacjach relacyjnej bazy danych, a nie z biblioteki. Dzięki użyciu kontrolki SqlDataSource możemy należy określić parametry połączenia bazy danych i zapytania ad hoc SQL lub procedury składowane do wykonywania do wstawiania, aktualizacji, usuwania i pobierania danych. SqlDataSource s `Select()`, `Insert()`, `Update()`, i `Delete()` metod, gdy wywoływany, nawiązać połączenie z określonej bazy danych i wystawiania odpowiednie zapytanie SQL. Jak na poniższym diagramie przedstawiono, te metody są nudną pracą nawiązywania połączenia z bazą danych, zapytania i zwraca wyniki.


![SqlDataSource służy jako serwer Proxy do bazy danych](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Rysunek 2**: SqlDataSource służy jako serwer Proxy do bazy danych


> [!NOTE]
> W tym samouczku skupimy się na pobieranie danych z bazy danych. W [Wstawianie, aktualizowanie i usuwania danych przy użyciu kontrolki SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) samouczków, zobaczymy, jak skonfigurować SqlDataSource umożliwiają wstawianie, aktualizowanie i usuwanie.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource i formanty AccessDataSource

Oprócz kontrolki SqlDataSource ASP.NET 2.0 obejmuje również kontrolkę AccessDataSource. Te dwa różne formanty prowadzić wielu programistów jesteś nowym użytkownikiem programu ASP.NET 2.0 podejrzewać, że formant AccessDataSource jest przeznaczona do pracy wyłącznie przy użyciu programu Microsoft Access, przy użyciu kontrolki SqlDataSource przeznaczona do pracy wyłącznie z programu Microsoft SQL Server. Gdy AccessDataSource jest przeznaczona do pracy z programu Microsoft Access, użyciu kontrolki SqlDataSource współpracuje z *wszelkie* relacyjnej bazy danych, który jest możliwy za pośrednictwem platformy .NET. Dotyczy to również wszelkich OLE DB lub ODBC-CLS magazyny danych, takich jak Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL i PostgreSQL, wiele innych.

Jedyny różnica między kontrolkami AccessDataSource i użyciu kontrolki SqlDataSource polega na tym, jak określono informacje o połączeniu. Kontrolka AccessDataSource wymaga po prostu ścieżkę pliku do pliku bazy danych programu Access. SqlDataSource, z drugiej strony, wymaga pełne parametry połączenia.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Krok 1: Tworzenie stron sieci Web z kontrolką SqlDataSource

Zanim zaczniemy, eksplorowanie sposób pracy bezpośrednio z bazy danych, przy użyciu kontrolki SqlDataSource, niech s najpierw Poświęć chwilę, do tworzenia stron ASP.NET w naszym projektu witryny sieci Web, który będziemy potrzebować dla tego samouczka i trzy dalej. Rozpocznij od dodania nowy folder o nazwie `SqlDataSource`. Następnie dodaj następujące strony ASP.NET do tego folderu, upewniając się skojarzyć każdą stronę z `Site.master` strona główna:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Dodawanie stron ASP.NET związane z kontrolką SqlDataSource samouczki](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Rysunek 3**: Dodawanie stron ASP.NET związane z kontrolką SqlDataSource samouczki


Podobnie jak w przypadku innych folderów `Default.aspx` w `SqlDataSource` folderu wyświetli listę samouczków w jego sekcji. Pamiętamy `SectionLevelTutorialListing.ascx` kontrolki użytkownika oferuje tę funkcję. W związku z tym, Dodaj ten formant użytkownika do `Default.aspx` , przeciągając go z poziomu Eksploratora rozwiązań na stronę s widoku projektu.


[![Dodaj formant użytkownika SectionLevelTutorialListing.ascx na Default.aspx](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Rysunek 4**: Dodaj `SectionLevelTutorialListing.ascx` kontrolki użytkownika do `Default.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


Wreszcie, Dodaj te cztery strony jako wpisy, aby `Web.sitemap` pliku. Ściślej mówiąc, Dodaj następujący kod po dodawania przycisków niestandardowych do kontrolek DataList i Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Po zaktualizowaniu `Web.sitemap`, Poświęć chwilę, aby wyświetlić witrynę sieci Web w samouczkach, za pośrednictwem przeglądarki. Menu po lewej stronie zawiera teraz elementy edytowanie, wstawianie i usuwanie samouczków.


![Mapa witryny zawiera teraz wpisy samouczki SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Rysunek 5**: mapy witryny zawiera teraz wpisy samouczki SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Krok 2: Dodawanie i konfigurowanie kontrolki SqlDataSource

Zacznij od otwarcia `Querying.aspx` stronie `SqlDataSource` folderu i przejdź do widoku projektu. Przeciągnij kontrolki SqlDataSource z przybornika do projektanta i ustaw jego `ID` do `ProductsDataSource`. Podobnie jak w przypadku elementu ObjectDataSource, SqlDataSource nie generuje żadnego wyniku renderowany i dlatego jest wyświetlany jako szary prostokąt na powierzchni projektowej. Aby skonfigurować SqlDataSource, kliknij łącze Konfiguruj źródła danych z kontrolką SqlDataSource tagu inteligentnego s.


![Kliknij pozycję Konfiguruj łącze do źródła danych z kontrolką SqlDataSource tagu inteligentnego s](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Rysunek 6**: kliknij pozycję Konfiguruj łącze do źródła danych z kontrolką SqlDataSource tagu inteligentnego s


Otwarte kreatora Konfigurowanie źródła danych SqlDataSource kontroli s. Chociaż kroki kreatora s różnią się od formantu ObjectDataSource s, jej celem jest taki sam sposób, aby podać szczegółowe informacje na temat sposobu pobierania, wstawianie, aktualizowanie i usuwanie danych za pośrednictwem źródła danych. Dla SqlDataSource to pociąga za sobą określenie podstawowej bazy danych do użycia i udostępnianie ad-hoc instrukcji SQL lub procedur składowanych.

Pierwszego kroku w Kreatorze nam wyświetla monit dotyczący bazy danych. Na liście rozwijanej znajdują się tych baz danych, w tym s aplikacji sieci web `App_Data` folder i te, które zostały dodane do węzła połączenia danych w Eksploratorze serwera. Ponieważ firma Microsoft już dodano ciąg połączenia dla `NORTHWIND.MDF` bazy danych w `App_Data` folder do naszego projektu s `Web.config` pliku, listy rozwijanej zawiera odwołanie do tego ciągu połączenia `NORTHWINDConnectionString`. Wybierz ten element z listy rozwijanej, a następnie kliknij przycisk Dalej.


![Z listy rozwijanej wybierz NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Rysunek 7**: Wybierz `NORTHWINDConnectionString` z listy rozwijanej


Po wybraniu bazy danych, kreator poprosi o podanie danych zwracanego przez zapytanie. Firma Microsoft można określić kolumn w tabeli lub widoku, aby zwrócić lub można wprowadzić niestandardową instrukcję SQL lub określić procedury przechowywanej. Można przełączać się między ten wybór za pośrednictwem Określ niestandardową instrukcję SQL lub procedur składowanych i określ kolumny z tabeli lub wyświetlanie przycisków radiowych.

> [!NOTE]
> W tym przykładzie pierwsze umożliwiają s, użyj kolumn Określ z poziomu opcji tabeli lub widoku. Utworzymy wróć do kreatora w dalszej części tego samouczka i Poznaj Określ niestandardową instrukcję SQL lub procedury składowanej opcji.


Rysunek 8 zawiera konfiguracji ekranu instrukcji Select po wybraniu kolumny wybierz przycisk radiowy tabeli lub widoku. Listy rozwijanej zawiera zbiór tabel i widoków w bazie danych Northwind, za pomocą wybranej tabeli lub kolumny s widoku wyświetlane na liście pole wyboru poniżej. W tym przykładzie zwróć umożliwiają s `ProductID`, `ProductName`, i `UnitPrice` kolumny z `Products` tabeli. Jak na rysunku nr 8 przedstawiono po tych wyborów Kreator pokazuje, wynikowa instrukcja SQL `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Zwróć dane z tabeli Produkty](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Rysunek 8**: dane zwrotne `Products` tabeli


Po skonfigurowaniu pracę kreatora Aby zwrócić `ProductID`, `ProductName`, i `UnitPrice` kolumny z `Products` tabelę, kliknij przycisk Dalej. Ten ekran końcowy zapewnia możliwość zbadania wyników kwerendy skonfigurowana w poprzednim kroku. Klikając przycisk Testuj zapytanie wykonuje skonfigurowanych `SELECT` instrukcji i wyświetla wyniki w siatce.


![Kliknij przycisk Testuj zapytanie do przeglądania zapytanie SELECT](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Rysunek 9**: kliknij przycisk Testuj zapytanie do przeglądu swojej `SELECT` zapytania


Aby zakończyć działanie kreatora, kliknij przycisk Zakończ.

Jak za pomocą kontrolki ObjectDataSource, Kreator s SqlDataSource jedynie przypisuje wartości do właściwości kontrolki s, a mianowicie [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) i [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) właściwości. Po ukończeniu kreatora, znaczników deklaratywne s kontrolki SqlDataSource powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString` Właściwość zawiera informacje na temat sposobu łączenia z bazą danych. Tej właściwości można przypisać wartość ciągu połączenia pełne, ustaloną lub wskazać parametrów połączenia w `Web.config`. Aby odwołać się w pliku Web.config wartość ciągu połączenia, należy użyć składni `<%$ expressionPrefix:expressionValue %>`. Zazwyczaj *expressionPrefix* jest ConnectionStrings i *expressionValue* jest nazwą ciągu połączenia w `Web.config` [ `<connectionStrings>` sekcji](https://msdn.microsoft.com/library/bf7sd233.aspx). Jednak można użyć składni z odwołaniem `<appSettings>` elementy lub zawartość z plików zasobów. Zobacz [Przegląd wyrażeń ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) Aby uzyskać więcej informacji na temat tej składni.

`SelectCommand` Właściwość określa ad-hoc instrukcji SQL lub procedurę składowaną można wykonać, aby zwrócić dane.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Krok 3: Dodawanie formantu sieci Web danych i powiązanie jej z kontrolką SqlDataSource

Po skonfigurowaniu SqlDataSource może być powiązana z danymi formantu sieci Web, takich jak GridView lub DetailsView. W tym samouczku umożliwiają wyświetlanie danych w kontrolce GridView s. Z przybornika przeciągnij GridView na stronę, który należy powiązać `ProductsDataSource` SqlDataSource, wybierając źródło danych z listy rozwijanej w tagu inteligentnego s GridView.


[![Dodaj GridView i powiązać kontrolki SqlDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Na rysunku nr 10**: Dodaj GridView i powiązać kontrolki SqlDataSource ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


Po wybraniu kontrolki SqlDataSource z listy rozwijanej w tagu inteligentnego s GridView był programu Visual Studio spowoduje automatyczne dodanie elementu BoundField lub CheckBoxField do kontrolki GridView dla każdej z kolumn, zwracany przez kontrolę źródła danych. Ponieważ SqlDataSource zwraca trzy bazy danych kolumn `ProductID`, `ProductName`, i `UnitPrice` istnieją trzy pola w widoku GridView.

Poświęć chwilę, aby skonfigurować s GridView trzech BoundFields. Zmiana `ProductName` s pola `HeaderText` Właściwość Nazwa produktu i `UnitPrice` s pole ceny. Również sformatować `UnitPrice` polu jako walutę. Po wprowadzeniu tych zmian, deklaratywne kontrolki GridView znaczników w s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Odwiedź tę stronę za pośrednictwem przeglądarki. Jak pokazano na ilustracji 11, widoku GridView wyświetla każdy produkt s `ProductID`, `ProductName`, i `UnitPrice` wartości.


[![Kontrolki GridView Wyświetla każdego produktu s ProductID ProductName wartości i cena jednostkowa](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Rysunek 11**: s GridView wyświetla każdy produkt `ProductID`, `ProductName`, i `UnitPrice` wartości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


Gdy odwiedzenia strony widoku GridView wywołuje kontrolę źródła danych s `Select()` metody. Gdy firma Microsoft była używana kontrolka ObjectDataSource, to tak zwane `ProductsBLL` klasy s `GetProducts()` metody. Dzięki użyciu kontrolki SqlDataSource, jednak `Select()` metoda nawiąże połączenie z określonej bazy danych i problemów `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, w tym przykładzie). SqlDataSource zwraca jego wyniki, które następnie wylicza widoku GridView, tworzenie wiersza w widoku GridView dla każdego rekordu bazy danych zwracane.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Funkcje kontroli danych wbudowane w sieci Web i użyciu kontrolki SqlDataSource

Ogólnie rzecz biorąc, funkcje związane z danymi kontrolki sieci Web, stronicowania, sortowanie, edytowanie, usuwanie, wstawianie i tak dalej są specyficzne dla danych kontrolki sieci Web i nie są zależne od kontroli źródła danych, które są używane. Oznacza to wbudowanej stronicowania, sortowanie, edytowanie i usuwanie, czy jest on powiązany z kontrolki ObjectDataSource lub SqlDataSource może korzystać z kontrolki GridView. Jednak niektóre dane funkcje kontroli sieci Web jest wielkość liter, do kontroli źródła danych, które są używane lub w konfiguracji kontroli s źródła danych.

Na przykład w [efektywne stronicowanie za pośrednictwem dużych ilości danych](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) samouczku omówiono, jak to zrobić, domyślnie logikę stronicowania danych sieci Web kontroluje naively zwraca *wszystkich* rekordy z podstawową źródła danych i następnie wyświetla odpowiedniego podzestawu rekordów, biorąc pod uwagę indeks bieżącej strony i liczbę rekordów do wyświetlenia na jednej stronie. Ten model jest bardzo mało wydajne, gdy stronicowanie wystarczająco duży zestaw wyników. Na szczęście kontrolki ObjectDataSource można skonfigurować do obsługi stronicowania niestandardowego, zwraca tylko dokładne podzestaw rekordów do wyświetlenia. Kontrolki SqlDataSource jednak brakuje właściwości Implementowanie stronicowania niestandardowego.

Innym subtlety z stronicowanie i sortowanie wynikającej z kontrolką SqlDataSource. Domyślnie dane zwrócone z kontrolką SqlDataSource można stronicowane lub sortowane za pośrednictwem widoku GridView. Aby to wykazać, sprawdź włączone stronicowanie i sortowanie Włączanie opcji w kontrolce GridView s tagu w `Querying.aspx` i sprawdź, czy działa zgodnie z oczekiwaniami.

Sortowanie i stronicowanie działa, ponieważ SqlDataSource pobiera dane bazy danych do zestawu danych typowaniem luźnym. Całkowita liczba rekordów zwróconych przez zapytanie ważnym aspektem na implementowanie stronicowania można ustalić z zestawu danych. Ponadto można sortować wyniki s zestawu danych za pomocą widoku danych. Te możliwości są automatycznie używane przy użyciu kontrolki SqlDataSource podczas żądania GridView stronicowane lub posortowanych danych.

SqlDataSource można skonfigurować do zwracania DataReader zamiast zestawu danych, zmieniając jego [ `DataSourceMode` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) z `DataSet` (ustawienie domyślne) do `DataReader`. Może być preferowane przy użyciu elementu DataReader w sytuacjach, w podczas przekazywania wyniki s SqlDataSource z istniejącym kodem, który oczekuje elementu DataReader. Ponadto ponieważ DataReaders obiektów znacznie prostsze niż zestawy danych, oferują lepszą wydajność. Po wprowadzeniu tej zmiany, jednak dane kontrolki sieci Web nie można sortować ani strony, ponieważ SqlDataSource nie może ustalić, ile rekordy są zwracane przez zapytanie, ani nie elementu DataReader oferują dowolnej techniki sortowanie zwracanych danych.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Krok 4: Za pomocą instrukcji języka SQL niestandardowych lub procedury składowanej

Podczas konfigurowania kontrolki SqlDataSource, zapytanie służy do zwracania danych można określić w jednej z dwóch metod jako niestandardową instrukcję SQL lub procedurę składowaną lub jako kolumny z istniejącej tabeli lub widoku. W kroku 2 zbadaliśmy wybranie kolumn z `Products` tabeli. Pozwól, s, Przyjrzyj się przy użyciu niestandardowych instrukcji języka SQL.

Dodaj inną kontrolkę GridView do `Querying.aspx` strony i wybrać opcję utworzenia nowego źródła danych z listy rozwijanej w tagu inteligentnego. Następnie należy wskazać, że dane będzie pobierany z bazy danych to spowoduje utworzenie nowej kontrolki SqlDataSource. Nazwij kontrolkę `ProductsWithCategoryInfoDataSource`.


![Tworzenie nowej kontrolki SqlDataSource o nazwie ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Rysunek 12**: Tworzenie nowej kontrolki SqlDataSource o nazwie `ProductsWithCategoryInfoDataSource`


Następny ekran pyta, czy nami, aby określić bazę danych. Wybierz ile My mieliśmy wstecz na rysunku 7 `NORTHWINDConnectionString` z listy rozwijanej i kliknij przycisk Dalej. W konfiguracji ekranu instrukcji Select wybierz Określ niestandardową instrukcję SQL lub procedury składowanej przycisk radiowy, a następnie kliknij przycisk Dalej. Spowoduje to wyświetlenie ekranu zdefiniować niestandardowe instrukcji lub procedur składowanych, który oferuje zakładki: SELECT, UPDATE, INSERT i DELETE. Na każdej karcie, można wprowadzić niestandardową instrukcję SQL do pola tekstowego lub wybierz procedurę składowaną z listy rozwijanej. W tym samouczku przyjrzymy się wprowadzanie niestandardowych instrukcji języka SQL; Następny samouczek obejmuje przykładowi, który korzysta z procedury składowanej.


![Wprowadź instrukcję SQL niestandardowych lub wybierz procedurę składowaną](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Rysunek 13**: wprowadź instrukcję SQL niestandardowych lub wybierz procedurę składowaną


Niestandardową instrukcję SQL można ręcznie wprowadzić w polu tekstowym lub być zbudowane w postaci graficznej, klikając przycisk konstruktora zapytań. Konstruktor kwerend lub pola tekstowego, użyj następującego zapytania, aby zwrócić `ProductID` i `ProductName` pola z `Products` tabeli, używając `JOIN` do pobrania produkt s `CategoryName` z `Categories` tabeli:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![Można graficznie skonstruować zapytania za pomocą konstruktora zapytań](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Rysunek 14**: możesz utworzyć graficznie zapytanie za pomocą konstruktora zapytań


Po określeniu zapytanie, kliknij przycisk Dalej, aby przejść do ekranu Testovat dotaz. Kliknij przycisk Zakończ, aby zakończyć działanie kreatora SqlDataSource.

Po ukończeniu kreatora, widoku GridView będzie mieć trzy BoundFields dodawanych do niego wyświetlanie `ProductID`, `ProductName`, i `CategoryName` kolumn zwróconych przez kwerendę i wynikowa w oznaczeniu deklaracyjnym następujące:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![Każdy identyfikator produktu s Nazwa kategorii nazwę i skojarzonych pokazuje, widoku GridView](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Rysunek 15**: Identyfikator kontrolki GridView pokazuje każdego produktu, nazwę i skojarzone nazwy kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>Podsumowanie

Jak wykonywać zapytania i wyświetlić dane przy użyciu kontrolki SqlDataSource widzieliśmy w ramach tego samouczka. Podobnie jak ObjectDataSource SqlDataSource służy jako serwer proxy, zapewniając deklaratywne podejście do uzyskiwania dostępu do danych. Właściwości Określ bazę danych, aby nawiązać połączenie i SQL `SELECT` Wyślij zapytanie w celu wykonania; mogą być określone w oknie właściwości lub za pomocą Kreatora konfigurowania źródła danych.

`SELECT` Przykłady zapytań zbadaliśmy w tym samouczku zwrócone wszystkie rekordy z określonego zapytania. Kontrolki SqlDataSource, jednak mogą obejmować `WHERE` klauzuli przy użyciu parametrów, których wartości są przypisane programistycznie lub są automatycznie pobierane z określonego źródła. Zajmiemy się jak tworzenie i używanie zapytań sparametryzowanych w następnym samouczku!

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Uzyskiwanie dostępu do danych z relacyjnej bazy danych](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource informacje o formancie](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Samouczki szybkiego startu platformy ASP.NET: Kontrolki SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Pliku Web.config `<connectionStrings>` — Element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Odwołanie do parametrów połączenia bazy danych](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Susan Connery Bernadette Leigh i David Suru. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [dalej](using-parameterized-queries-with-the-sqldatasource-vb.md)
