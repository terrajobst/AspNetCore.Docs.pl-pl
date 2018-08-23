---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: Efektywne stronicowanie dużych ilości danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Domyślna opcja stronicowania kontrolki prezentacji danych jest nieodpowiednia, podczas pracy z dużymi ilościami danych, jako jego podstawowy pobierania kontroli źródła danych...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 5870b9ac2af7d9df927edd8230334dee4885c6a1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753382"
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>Efektywne stronicowanie dużych ilości danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe) lub [Pobierz plik PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> Domyślna opcja stronicowania kontrolki prezentacji danych nie nadaje się podczas pracy z dużymi ilościami danych, zgodnie z jego podstawowej kontroli źródła danych pobiera wszystkie rekordy, mimo że jest wyświetlany tylko podzestaw danych. W takiej sytuacji firma Microsoft musi włączyć z niestandardowymi stronicowania.


## <a name="introduction"></a>Wprowadzenie

Tak jak Omówiliśmy to w poprzednim samouczku, można zaimplementować stronicowania w jeden z dwóch sposobów:

- **Domyślne stronicowania** można zaimplementować po prostu, zaznaczając opcję włączenia stronicowania danych formantu sieci Web s tagów inteligentnych; jednak zawsze wtedy, gdy wyświetlania strony danych, pobiera kontrolki ObjectDataSource *wszystkich* rekordów, nawet Mimo że tylko część z nich są wyświetlane na stronie
- **Niestandardowe stronicowania** poprawia wydajność domyślna stronicowania przez pobieranie tylko te rekordy z bazy danych, która należy do wyświetlenia określonej strony danych wymaganych przez użytkownika; jednak stronicowania niestandardowego wymaga nieco więcej pracy w celu zaimplementowania niż domyślna stronicowania

Ze względu na łatwość implementacji wyboru tylko pola wyboru i ponownie gotowe! stronicowanie domyślny jest atrakcyjną opcję. Jej podejście ve na podczas pobierania wszystkich rekordów, jednak sprawia, że wybór nieprzekonywających podczas stronicowanie wystarczająco duże ilości danych lub dla witryn z wieloma równoczesnymi użytkownikami. W takiej sytuacji firma Microsoft należy go włączyć z niestandardowymi stronicowania w celu zapewnienia elastyczny system.

Wyzwanie stronicowania niestandardowego jest możliwość Napisz zapytanie, które zwraca dokładny zestaw rekordów niezbędnych dla określonej strony danych. Na szczęście usługa Microsoft SQL Server 2005 zapewnia nowe słowo kluczowe wyniki klasyfikacji, który pozwala nam napisać zapytanie, które pozwala na efektywne pobieranie odpowiednie podzestaw rekordów. W tym samouczku Zobaczymy się, jak zaimplementować niestandardowy stronicowania w kontrolce GridView za pomocą tego nowego słowa kluczowego SQL Server 2005. Interfejs użytkownika dla niestandardowych stronicowania jest identyczna jak domyślne stronicowanie, przechodzenie z jednej strony, aby dalej korzystać stronicowania niestandardowego może być wiele rzędów szybciej niż domyślna stronicowania.

> [!NOTE]
> Przyrost wydajności dokładnie ujawnione przez niestandardowe stronicowania zależy od łączną liczbą rekordów są stronicowane za pośrednictwem i obciążenia są umieszczone na serwerze bazy danych. Na końcu tego samouczka rozpatrzymy niektóre nierównej metryki, które pokazują korzyści w wydajności uzyskane za pośrednictwem stronicowania niestandardowego.


## <a name="step-1-understanding-the-custom-paging-process"></a>Krok 1: Opis niestandardowego procesu stronicowania

Jeśli stronicowanie danych, wyświetlanych na stronie rekordów dokładne zależą strony są żądane dane i liczba rekordów wyświetlanych na stronie. Na przykład Wyobraź sobie, że Chcieliśmy stronie za pośrednictwem 81 produktów, wyświetlając 10 produktów na stronie. Podczas wyświetlania na pierwszej stronie, d chcemy produktów od 1 do 10; Podczas wyświetlania na drugiej stronie mamy d zainteresować 11 20 za pomocą produktów i tak dalej.

Istnieją trzy zmienne, które wymuszają rekordy muszą być pobierane i jak ma być renderowany interfejsu stronicowania:

- **Rozpocznij indeks wiersza** indeksu pierwszego wiersza na stronie danych do wyświetlenia; może to indeks obliczane przez pomnożenie indeks strony przez rekordów do wyświetlenia na jednej stronie i dodanie jednego. Na przykład, jeśli stronicowanie rekordów 10 w czasie, dla pierwszej strony (której indeks strony wynosi 0), indeks wiersza Start wynosi 0 \* 10 + 1 lub 1; dla drugiej strony (której indeks strony wynosi 1) indeks wiersza Start jest 1 \* 10 + 1 , lub 11.
- **Maksymalna liczba wierszy** maksymalną liczbę rekordów do wyświetlenia na jednej stronie. Ta zmienna jest określana jako maksymalna liczba wierszy, ponieważ dla ostatniej strony może być mniejszą liczbę rekordów zwrócił niż rozmiar strony. Na przykład stronicowanie 81 produktów 10 rekordy na każdej stronie, dziewiąty i ostatnia strona ma tylko jeden rekord. Brak strony wyświetli jednak więcej rekordów niż wartość maksymalna liczba wierszy.
- **Łączna liczba rekordów** całkowita liczba rekordów, które są stronicowane za pośrednictwem. Podczas tego t zmiennej nie jest potrzebne do określenia rekordów do pobrania dla danej strony, jego dyktowanie interfejsu stronicowania. Na przykład w przypadku produktów 81 są stronicowane za pośrednictwem interfejsu stronicowania wie, do wyświetlenia dziewięć numery stron w interfejsie użytkownika stronicowania.

Za pomocą stronicowania domyślny indeks wiersza Start jest obliczana jako iloczyn indeks strony i rozmiar strony, oraz jedną, maksymalna liczba wierszy jest po prostu rozmiaru strony. Ponieważ domyślne stronicowania pobiera wszystkie rekordy z bazy danych podczas renderowania dowolnej stronie danych i indeksów dla każdego wiersza jest znany, sprawiając, że przenoszenie do wiersza Rozpocznij indeks wiersza jest prostym zadaniem. Ponadto, łączna liczba rekordów jest łatwo dostępne, ponieważ s po prostu liczbę rekordów w tabeli DataTable (lub dowolnego obiektu jest używana do przechowywania wyników bazy danych).

Biorąc pod uwagę zmienne Rozpocznij indeks wiersza i maksymalna liczba wierszy, implementacja niestandardowa stronicowania musi zwrócić tylko dokładne podzestaw rekordów uruchomienie na indeks wiersza Start wynosi maksymalna liczba wierszy liczba rekordów po tym. Niestandardowe stronicowania zapewnia wyzwanie z dwóch powodów:

- Firma Microsoft musi być w stanie efektywnie skojarzyć indeks wiersza z każdego wiersza w wszystkie dane są stronicowane za pośrednictwem, dzięki czemu możemy rozpocząć zwracanie rekordów dla podanego indeksu Uruchom wiersz
- Należy podać całkowita liczba rekordów, które są stronicowane za pośrednictwem

W dwóch następnych krokach zostaną omówione skrypt SQL niezbędne do odpowiadania na tych dwóch wyzwań. Oprócz skrypt SQL możemy również należy implementować metody DAL i LOGIKI.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Krok 2: Zwracanie całkowita liczba rekordów, które są stronicowane za pośrednictwem

Zanim omówiony sposób pobierania dokładne podzestaw rekordów dla strony są wyświetlane, chętnie s Pierwsze spojrzenie na sposób zwracania całkowita liczba rekordów, które są stronicowane za pośrednictwem. Te informacje są potrzebne, aby można było prawidłowo skonfigurować interfejs użytkownika stronicowania. Całkowita liczba rekordów zwróconych przez zapytanie SQL w szczególności można uzyskać za pomocą [ `COUNT` funkcję agregacji](https://msdn.microsoft.com/library/ms175997.aspx). Na przykład, aby określić, całkowita liczba rekordów w `Products` tabeli, możemy użyć następującej kwerendy:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

Pozwól s dodać metodę do naszych DAL, która zwraca te informacje. W szczególności, utworzymy warstwę DAL metodę o nazwie `TotalNumberOfProducts()` , który jest wykonywany `SELECT` instrukcji przedstawionych powyżej.

Zacznij od otwarcia `Northwind.xsd` wpisane plik zestawu danych w `App_Code/DAL` folderu. Następnie kliknij prawym przyciskiem myszy `ProductsTableAdapter` w Projektancie i wybierz polecenie Dodaj zapytanie. Ponieważ ve w poprzednich samouczkach to umożliwi nam Dodaj nową metodę z warstwą dal, po wywołaniu, wykona określoną instrukcję SQL lub procedury składowanej. Podobnie jak w przypadku naszej metody TableAdapter w poprzednich samouczkach, ten jeden zdecydować się na używanie instrukcji SQL zapytań ad-hoc.


![Używanie instrukcji SQL zapytań Ad-Hoc](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**Rysunek 1**: Użyj instrukcji SQL zapytań Ad-Hoc


Na następnym ekranie możemy określić, jakiego rodzaju zapytanie w celu utworzenia. Ponieważ to zapytanie będzie zwracać wartość pojedynczą, skalarną, całkowita liczba rekordów w `Products` wybierz tabelę `SELECT` zwraca opcję wartość wystąpieniu.


![Konfigurowanie zapytania, aby użyć instrukcji SELECT, która zwraca pojedynczą wartość](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**Rysunek 2**: Skonfiguruj zapytania, aby użyć instrukcji SELECT, która zwraca pojedynczą wartość


Po wskazujący typ zapytanie do użycia, możemy następnie określ zapytanie.


![Użyj wybierz COUNT(*) z kwerendy produktów](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**Rysunek 3**: Użyj wybierz liczby (\*) FROM produktów zapytania


Na koniec należy określić nazwę metody. Jak wyżej, pozwól s Użyj `TotalNumberOfProducts`.


![Nadaj nazwę TotalNumberOfProducts DAL — metoda](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**Rysunek 4**: Nazwa TotalNumberOfProducts DAL — metoda


Po kliknięciu przycisku Zakończ, Kreator doda `TotalNumberOfProducts` metody z warstwą dal. Skalarna zwracanych metody w warstwy DAL zwracają typy dopuszczające wartości null, w przypadku, gdy wynikiem zapytania SQL, który jest `NULL`. Nasze `COUNT` kwerendy, jednak zawsze zwróci innej niż`NULL` wartość; niezależnie od tego, metoda DAL zwraca liczbę całkowitą typu dopuszczającego wartość null.

Oprócz metody DAL również należy do metody w LOGIKI. Otwórz `ProductsBLL` klasy plików i Dodaj `TotalNumberOfProducts` metodę, która po prostu wywołuje Widok s DAL `TotalNumberOfProducts` metody:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s `TotalNumberOfProducts` metoda zwraca liczbę całkowitą typu dopuszczającego wartość null; Jednakże, firma Microsoft ve utworzone `ProductsBLL` klasy s `TotalNumberOfProducts` metodę, tak że zwraca standardowy liczbę całkowitą. Dlatego musimy mieć `ProductsBLL` klasy s `TotalNumberOfProducts` część wartości liczby całkowitej dopuszczającego wartość null, zwracany przez DAL s zwracany przez metodę `TotalNumberOfProducts` metody. Wywołanie `GetValueOrDefault()` zwraca wartość całkowitą dopuszczającego wartość null, jeśli istnieje; Jeśli nullable liczba całkowita jest `null`, jednak funkcja zwraca wartość całkowitą domyślna to 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Krok 3: Zwracanie dokładne podzestaw rekordów

Naszym kolejnym krokiem jest utworzenie metody DAL i LOGIKI, które akceptują indeks wiersza Start, a maksymalna liczba wierszy zmienne omówionych powyżej i zwracają odpowiednie rekordy. Zanim do tego, umożliwiają s najpierw Przyjrzyj się potrzebne skryptu SQL. Żądanie połączonego z nami to, że firma Microsoft musi być w stanie efektywnie przypisać indeksu do każdego wiersza w całej wyniki są stronicowane za pośrednictwem tak, aby firma Microsoft może zwracać tylko te rekordy, uruchomienie na indeks wiersza Start (maksymalnie rekordów maksymalną liczbę rekordów).

To nie jest trudne, jeśli istnieje już kolumna w tabeli bazy danych, która służy jako indeks wiersza. Na pierwszy rzut oka może uważamy, że `Products` tabeli s `ProductID` pola będą wystarczające jako pierwszy produkt ma `ProductID` 1, drugi 2, i tak dalej. Jednak usunięcie produktu pozostawia przerwy w sekwencji, rozpoczynającą tego podejścia.

Istnieją dwie techniki ogólne używane do wydajnego kojarzenia indeks wiersza z danymi do przeglądania, umożliwiając w ten sposób dokładne podzestaw rekordów do pobrania:

- **Za pomocą programu SQL Server 2005 s `ROW_NUMBER()` — słowo kluczowe** jesteś nowym użytkownikiem programu SQL Server 2005, `ROW_NUMBER()` — słowo kluczowe kojarzy klasyfikacji z każdego rekordu zwrócone według kolejności. Ta klasyfikacja może służyć jako indeks wiersza dla każdego wiersza.
- **Za pomocą zmiennej tabeli i `SET ROWCOUNT`**  programu SQL Server [ `SET ROWCOUNT` instrukcji](https://msdn.microsoft.com/library/ms188774.aspx) może służyć do określenia, ile łącznie rekordów zapytanie ma być przetwarzana przed przerwaniem; [tabeli zmienne](http://www.sqlteam.com/item.asp?ItemID=9454) są zmienne lokalne języka T-SQL, które mogą zawierać dane tabelaryczne akin do [tabele tymczasowe](http://www.sqlteam.com/item.asp?ItemID=2029). Ta metoda działa równie dobrze z programu Microsoft SQL Server 2005 i SQL Server 2000 (natomiast `ROW_NUMBER()` podejście działa tylko w przypadku programu SQL Server 2005).  
  
  W tym miejscu chodzi o to, aby utworzyć zmienną tabeli, która ma `IDENTITY` kolumny i kolumny kluczy podstawowych w tabeli, którego dane są są stronicowane za pośrednictwem. Następnie zawartość tabeli, którego dane są są stronicowane za pośrednictwem jest zrzucany do zmiennej tabeli, w tym samym kojarzenie indeks wiersza sekwencyjnego (za pośrednictwem `IDENTITY` kolumny) dla każdego rekordu w tabeli. Po wprowadzeniu zmiennej tabeli `SELECT` instrukcji w zmiennej tabeli łączone z tabeli podstawowej, mogą być wykonywane na nagraniach określonych rekordów. `SET ROWCOUNT` Instrukcja jest używane do inteligentnie Ogranicz liczbę rekordów, które trzeba można utworzyć zrzutu do zmiennej tabeli.  
  
  Takie zwiększenie efektywności podejście s jest oparty na numer strony są żądane jako `SET ROWCOUNT` wartość jest przypisywana wartość Start indeks wiersza, a także maksymalna liczba wierszy. Jeśli stronicowanie parzystym wierszom stron, takich jak pierwsze kilka stron danych to podejście jest bardzo wydajny. Jednak wskazuje wydajności stronicowania przypominającej domyślne podczas pobierania strony blisko końca.

W tym samouczku implementuje niestandardowe za pomocą stronicowania `ROW_NUMBER()` — słowo kluczowe. Aby uzyskać więcej informacji na temat używania zmiennej tabeli i `SET ROWCOUNT` techniki, zobacz [więcej efektywną metodą stronicowanie za pośrednictwem dużych zestawów wyników](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

`ROW_NUMBER()` — Słowo kluczowe skojarzone klasyfikacji z każdego rekordu zwrócone w kolejności określonej przy użyciu następującej składni:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` Zwraca wartość liczbową, która określa rangę dla każdego rekordu w odniesieniu do wskazanej kolejności. Na przykład aby zobaczyć rangi dla każdego produktu, uporządkowane od najbardziej kosztowne do najmniejszej, można Stosujemy następujące zapytanie:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

Rysunek 5. Pokazuje to zapytanie s wyniki podczas uruchamiania w oknie zapytania w programie Visual Studio. Należy pamiętać, że produkty są uporządkowane według ceny, wraz z rangę cena dla każdego wiersza.


![Ranga cena znajduje się dla każdego rekordu zwracane](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**Rysunek 5**: ranga cena znajduje się dla każdego rekordu zwracane


> [!NOTE]
> `ROW_NUMBER()` jest tylko jeden z wielu nowych funkcji Klasyfikacja dostępnych w programie SQL Server 2005. Aby uzyskać bardziej szczegółowe omówienie `ROW_NUMBER()`, wraz z innych funkcji Klasyfikacja, przeczytaj [zwracanie wyników randze spośród wszystkich dokumentów przy użyciu programu Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Gdy klasyfikacja wyniki wg określonego `ORDER BY` kolumny w `OVER` — klauzula (`UnitPrice`, w powyższym przykładzie), programu SQL Server należy sortować wyniki. To to szybka operacja, w przypadku indeksu klastrowanego za pośrednictwem kolumn na liście wyników są szeregowane, lub jeśli jest pokryciem indeksu, ale może być bardziej kosztowne inaczej. Aby poprawić wydajność zapytań wystarczająco duże, Rozważ dodanie indeksu nieklastrowanego dla kolumny, według której wyniki są uporządkowane według. Zobacz [funkcji Klasyfikacja i wydajności w programie SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) uzyskać bardziej szczegółowy widok zagadnienia związane z wydajnością.

Informacje o klasyfikacji, zwrócone przez `ROW_NUMBER()` nie może być używany bezpośrednio `WHERE` klauzuli. Jednak tabeli pochodnej może służyć do zwrócenia `ROW_NUMBER()` wynik, który następnie może występować w `WHERE` klauzuli. Na przykład, następujące zapytanie używa tabeli pochodnej do zwrócenia z właściwościami ProductName i UnitPrice kolumny, wraz z `ROW_NUMBER()` wyników, a następnie używa `WHERE` klauzulę, aby zwracać tylko te produkty której pozycję cena jest między 11 i 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

Rozszerzanie nieco dodatkowo to pojęcie, firma Microsoft mogą korzystać z tego podejścia do pobrania określonej strony danych podane odpowiednie wartości Rozpocznij indeks wiersza i maksymalna liczba wierszy:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> Jak firma Microsoft zostanie wyświetlony później w tym samouczku *`StartRowIndex`* dostarczonych przez kontrolki ObjectDataSource są indeksowane począwszy od zera, natomiast `ROW_NUMBER()` wartości zwracanej przez program SQL Server 2005 są indeksowane począwszy od 1. W związku z tym `WHERE` klauzula zwraca te rekordy, których `PriceRank` jest większa niż *`StartRowIndex`* i mniejsza niż lub równa *`StartRowIndex`*  +  *`MaximumRows`*.


Teraz tego możemy ve omówiono sposób `ROW_NUMBER()` mogą być używane do pobierania na danej stronie dane podane wartości Rozpocznij indeks wiersza i maksymalna liczba wierszy, teraz musisz zaimplementować tę logikę jako metody DAL i logiki warstwy Biznesowej.

Podczas tworzenia tego zapytania, że należy zdecydować, to porządkowanie za pomocą której wyniki zostanie wyznaczona ranga; Pozwól s posortować produkty według nazwy w kolejności alfabetycznej. Oznacza to, że za pomocą niestandardowych implementacji stronicowania, w tym samouczku firma Microsoft nie będzie można utworzyć raport niestandardowy stronicowane, niż można również sortować. W następnym samouczku, zobaczymy, jak można podać takich funkcji.

W poprzedniej sekcji utworzyliśmy metoda DAL jako instrukcji SQL zapytań ad-hoc. Niestety, analizator składni języka T-SQL w programie Visual Studio używane przez t TableAdapter Kreator, takich jak `OVER` składnią używaną przez `ROW_NUMBER()` funkcji. W związku z tym jako procedurę przechowywaną, firma Microsoft należy utworzyć w tej metody warstwy DAL. Wybierz Server Explorer z menu Widok (lub trafień Ctrl + Alt + S) i rozwiń opcję `NORTHWND.MDF` węzła. Aby dodać nową procedurę składowaną, kliknij prawym przyciskiem myszy w węźle procedur składowanych, a następnie kliknij przycisk Dodaj nową procedurę przechowywaną (patrz rysunek 6).


![Dodaj nową procedurę składowaną dla stronicować produktów](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**Rysunek 6**: Dodaj nową procedurę składowaną dla stronicować produktów


Tę procedurę składowaną, należy zaakceptować dwóch parametrów wejściowych integer - `@startRowIndex` i `@maximumRows` i użyj `ROW_NUMBER()` funkcja uporządkowane według `ProductName` pola, zwracanie tylko tych wierszy, które jest większa niż określona `@startRowIndex` i mniejsza niż lub równa `@startRowIndex`  +  `@maximumRow` s. Wprowadź następujący skrypt do nowej procedury składowanej, a następnie kliknij przycisk Zapisz, aby dodać procedurę składowaną w bazie danych.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

Po utworzeniu procedury składowanej, Poświęć chwilę, aby przetestować działanie. Kliknij prawym przyciskiem myszy `GetProductsPaged` procedury składowanej nazwy w Eksploratorze serwera i wybierz opcję wykonania. Program Visual Studio wyświetlany jest monit dla parametrów wejściowych `@startRowIndex` i `@maximumRow` s (zobacz rysunek 7). Wypróbuj różne wartości i sprawdź wyniki.


![Wprowadź wartość w @startRowIndex i @maximumRows parametrów](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>Rysunek 7</strong>: wprowadź wartość w @startRowIndex i @maximumRows parametrów


Po wybierając te wprowadzanie wartości parametrów, w oknie danych wyjściowych zostaną wyświetlone wyniki. Rysunek 8 przedstawia wyniki podczas przekazywania w 10 dla obu `@startRowIndex` i `@maximumRows` parametrów.


[![Zwracane są rekordy, zostanie wyświetlony w drugiej strony danych](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**Rysunek 8**: zwracane są rekordy, zostanie wyświetlony w drugiej strony danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


Dzięki temu przechowywane procedury tworzenia, możemy ponownie gotowe do utworzenia `ProductsTableAdapter` metody. Otwórz `Northwind.xsd` wpisana zestawu danych, kliknij prawym przyciskiem myszy w `ProductsTableAdapter`, a następnie wybierz opcję Dodaj zapytanie. Zamiast tworzenia zapytania przy użyciu instrukcji SQL zapytań ad-hoc, utwórz ją za pomocą istniejącą procedurę składowaną.


![Utwórz metodę DAL przy użyciu istniejącą procedurę składowaną](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**Rysunek 9**: Utwórz metodę DAL przy użyciu istniejącą procedurę składowaną


Następnie możemy monit wybierz procedurę składowaną do wywołania. Wybierz `GetProductsPaged` przechowywane procedury z listy rozwijanej.


![Wybierz GetProductsPaged przechowywane procedury z listy rozwijanej](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**Na rysunku nr 10**: Wybierz GetProductsPaged przechowywane procedury z listy rozwijanej


Następny ekran następnie zapyta, jakiego rodzaju dane zwracane przez procedurę składowaną: dane tabelaryczne, pojedynczej wartości lub brak wartości. Ponieważ `GetProductsPaged` procedurę składowaną można zwrócić wiele rekordów, wskazują, że zwraca ona dane tabelaryczne.


![Wskazuje, że procedura składowana ma zwracać dane tabelaryczne](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**Rysunek 11**: wskazuje, że procedura składowana ma zwracać dane tabelaryczne


Na koniec nazwiskami metod, które zostały utworzone. Podobnie jak w przypadku naszej poprzednich samouczkach Przejdź dalej i tworzenie metod za pomocą obu wypełnienia DataTable i zwraca DataTable. Nazwa pierwszej metody `FillPaged` , a druga `GetProductsPaged`.


![Nazwa FillPaged metod i GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**Rysunek 12**: Nazwa FillPaged metod i GetProductsPaged


Ponadto utworzyć metodę warstwy DAL do zwrócenia na danej stronie produktów, musimy również udostępniają takie funkcje, w LOGIKI. Podobna do metody DAL s LOGIKI metoda GetProductsPaged musi zaakceptować dwie liczby całkowitej danych wejściowych, Rozpocznij indeks wiersza i maksymalna liczba wierszy, a musi zwracać tylko te rekordy, które mieszczą się w określonym zakresie. Utwórz metodę LOGIKI w klasie ProductsBLL, że jedynie wywołaniami dół s DAL GetProductsPaged metody, w następujący sposób:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

Można użyć dowolnej nazwy parametrów wejściowych metody s LOGIKI, ale, jak firma Microsoft pojawi się wkrótce, wybór użycia `startRowIndex` i `maximumRows` oszczędza nam dodatkowy bit pracy podczas konfigurowania elementu ObjectDataSource do używania tej metody.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Krok 4: Konfigurowanie kontrolki ObjectDataSource do użycia niestandardowego stronicowania

Za pomocą metod LOGIKI i warstwy DAL do uzyskiwania dostępu do konkretnego podzbioru rekordy pełne możemy ponownie gotowe do utworzenia w kontrolce GridView kontrolować tej strony za pomocą jego rekordy przy użyciu niestandardowych stronicowania. Zacznij od otwarcia `EfficientPaging.aspx` stronie `PagingAndSorting` folderu, na stronie Dodaj GridView i skonfigurować go do używania nowej kontrolki ObjectDataSource. W przeszłości materiały szkoleniowe, mieliśmy często skonfigurowane do używania kontrolki ObjectDataSource `ProductsBLL` klasy s `GetProducts` metody. Tym razem jednak chcemy do użycia `GetProductsPaged` metody zamiast tego należy od `GetProducts` metoda zwraca *wszystkich* produktów w bazie danych natomiast `GetProductsPaged` zwraca konkretnego podzestawu rekordów.


![Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductsPaged ProductsBLL klasy s](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**Rysunek 13**: Konfigurowanie kontrolki ObjectDataSource przy użyciu metody GetProductsPaged ProductsBLL klasy s


Ponieważ możemy ponownie tworzenie GridView tylko do odczytu Poświęć chwilę, aby ustawić listy rozwijanej metody INSERT, UPDATE i usuwanie kart (Brak).

Następnie Kreator ObjectDataSource nam monituje o podanie źródła `GetProductsPaged` metoda s `startRowIndex` i `maximumRows` wprowadzanie wartości parametrów. Te parametry wejściowe zostaną rzeczywiście ustawione przez widoku GridView automatycznie więc po prostu pozostaw źródłowy zestaw None i kliknij przycisk Zakończ.


![Pozostaw źródła parametr wejściowy, jak None](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**Rysunek 14**: pozostaw źródła parametr wejściowy, jak None


Po zakończeniu pracy Kreatora ObjectDataSource widoku GridView będzie zawierać elementu BoundField lub CheckBoxField dla każdego pola danych produktu. Możesz dostosować wygląd s GridView, zgodnie z potrzebami. Czy mogę ve zgody, aby wyświetlić tylko `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, i `UnitPrice` BoundFields. Ponadto należy skonfigurować GridView do obsługi stronicowania, zaznaczając pole wyboru Włącz stronicowania w jego tagu inteligentnego. Po wprowadzeniu tych zmian oznaczeniu deklaracyjnym kontrolkami GridView i kontrolki ObjectDataSource powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

W przypadku odwiedzenia strony za pośrednictwem przeglądarki widoku GridView jest jednak którego ma zostać odnaleziona.


![Kontrolki GridView jest niewidoczne](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**Rysunek 15**: GridView jest niewidoczne


Kontrolki GridView brakuje ponieważ kontrolki ObjectDataSource jest obecnie używany przez 0 jako wartości dla obu `GetProductsPaged` `startRowIndex` i `maximumRows` parametrów wejściowych. W związku z tym wynikowe zapytanie SQL zwraca żadnych rekordów i w związku z tym nie jest wyświetlana widoku GridView.

Aby rozwiązać ten problem, należy skonfigurować ObjectDataSource do użycia niestandardowego stronicowania. Można to zrobić w poniższych krokach:

1. **Ustaw ObjectDataSource s `EnablePaging` właściwości `true`**  wskazuje ObjectDataSource, jaki musi minąć do `SelectMethod` dwa dodatkowe parametry: umożliwiających określenie indeks wiersza Start ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)), a drugi do określenia maksymalna liczba wierszy ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Ustaw ObjectDataSource s `StartRowIndexParameterName` i `MaximumRowsParameterName` odpowiednio do właściwości** `StartRowIndexParameterName` i `MaximumRowsParameterName` właściwości z nazwiskami parametry wejściowe przekazywane do `SelectMethod` potrzeby stronicowania niestandardowych. Domyślnie, te nazwy parametru są `startIndexRow` i `maximumRows`, co jest dlaczego, tworząc `GetProductsPaged` metody w LOGIKI, I użyć tych wartości dla parametrów wejściowych. Jeśli została wybrana opcja używania nazwy różnych parametrów dla s LOGIKI `GetProductsPaged` metody takie jak `startIndex` i `maxRows`dla przykładu, należy ustawić ObjectDataSource s `StartRowIndexParameterName` i `MaximumRowsParameterName` właściwości odpowiednio (na przykład startIndex dla `StartRowIndexParameterName` i maksymalna liczba wierszy dla `MaximumRowsParameterName`).
3. **Ustaw ObjectDataSource s [ `SelectCountMethod` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) na nazwę metody, która zwraca całkowita liczba z rekordów jest stronicowanej za pośrednictwem (`TotalNumberOfProducts`)** pamiętamy `ProductsBLL` klasy s `TotalNumberOfProducts`metoda zwróci wartość całkowita liczba rekordów, które są stronicowane za pomocą metody DAL, który jest wykonywany `SELECT COUNT(*) FROM Products` zapytania. Te informacje są potrzebne przez kontrolki ObjectDataSource celu została poprawnie renderowana interfejsu stronicowania.
4. **Usuń `startRowIndex` i `maximumRows` `<asp:Parameter>` elementów z kontrolki ObjectDataSource s w oznaczeniu deklaracyjnym** podczas konfigurowania elementu ObjectDataSource z kreatorem, Visual Studio automatycznie dodane dwa `<asp:Parameter>` elementów Aby uzyskać `GetProductsPaged` metoda s parametrów wejściowych. Ustawiając `EnablePaging` do `true`, te parametry zostaną automatycznie przekazane; Jeśli pojawiają się również w składni deklaratywnej, kontrolki ObjectDataSource spróbuje przekazać *cztery* parametry `GetProductsPaged` — metoda i dwa parametry `TotalNumberOfProducts` metody. Jeśli nie pamiętasz usunąć te `<asp:Parameter>` elementów, podczas wyświetlania strony za pośrednictwem przeglądarki, otrzymasz komunikat o błędzie, takich jak: *ObjectDataSource "ObjectDataSource1" nie można odnaleźć metody nieogólnego TotalNumberOfProducts, który ma Parametry: startRowIndex, maximumRows*.

Po wprowadzeniu tych zmian, składni deklaratywnej s ObjectDataSource powinien wyglądać następująco:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

Należy pamiętać, że `EnablePaging` i `SelectCountMethod` zostały ustawione właściwości i `<asp:Parameter>` elementy zostały usunięte. Rysunek 16 przedstawiono zrzut ekranu przedstawiający okno właściwości po tych zmian.


![Aby użyć niestandardowego stronicowania, należy skonfigurować formantu ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**Rysunek 16**: Aby użyć niestandardowego stronicowania, należy skonfigurować formantu ObjectDataSource


Po wprowadzeniu tych zmian, odwiedź tę stronę za pośrednictwem przeglądarki. Powinien zostać wyświetlony 10 z wymienionych poniżej produktów, uporządkowana w kolejności alfabetycznej. Poświęć chwilę, aby przejść przez jedną stronę danych w czasie. W trakcie Brak visual różnicy z perspektywy użytkownika końcowego s stronicowania domyślne i niestandardowe stronicowania niestandardowe wydajniej stronicowania między stronami w ramach dużych ilości danych, ponieważ pobiera tylko te rekordy, które muszą być wyświetlany dla danej strony.


[![Dane, Zamówione według produktu s nazwa, jest stronicowania niestandardowe stronicowanej przy użyciu](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**Rysunek 17**: danych zamówione według produktu s nazwa, jest stronicowania niestandardowe stronicowanej przy użyciu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> Za pomocą niestandardowych stronicowania strony liczba wartości zwracanej przez ObjectDataSource s `SelectCountMethod` znajduje się w stanie widoku GridView s. Inne zmienne GridView `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` kolekcji i tak dalej, które są przechowywane w *kontrolowany stan*, który jest trwały bez względu na wartość GridView s `EnableViewState` Właściwość. Ponieważ `PageCount` wartość jest utrwalony między ogłaszania zwrotnego za pomocą stan widoku, korzystając z interfejsu stronicowania, który zawiera link umożliwiający przejście do ostatniej strony, konieczne jest włączenie stan widoku GridView s. (Jeśli interfejsu stronicowania nie zawiera bezpośredni link do ostatniej strony, a następnie możesz wyłączyć wyświetlanie stanu.)


Klikając łącze do strony ostatnich powoduje odświeżenie strony i powoduje, że GridView, aby zaktualizować jego `PageIndex` właściwości. Po kliknięciu łącze do strony ostatnich widoku GridView przypisuje jej `PageIndex` właściwość z wartością, jeden mniejsza od jego `PageCount` właściwości. Widok stanu wyłączone `PageCount` wartości są tracone w ogłaszania zwrotnego i `PageIndex` zamiast tego przypisuje się wartość maksymalna liczba całkowita. Następnie próbuje określić początkowy indeks wiersza przez pomnożenie widoku GridView `PageSize` i `PageCount` właściwości. Skutkuje to `OverflowException` ponieważ produkt przekracza rozmiar maksymalny dozwolony liczby całkowitej.

## <a name="implement-custom-paging-and-sorting"></a>Implementowanie niestandardowego stronicowanie i sortowanie

Nasz bieżący implementację niestandardową stronicowania wymaga kolejności za pomocą którego dane są stronicowane za pośrednictwem statycznie podczas tworzenia `GetProductsPaged` procedury składowanej. Jednak użytkownik może zostały zanotowane czy tagu inteligentnego s GridView zawiera włączyć sortowanie pola wyboru oprócz opcję włączenia stronicowania. Niestety dodanie obsługi sortowania w kontrolce GridView o naszej bieżącej niestandardowych implementacji stronicowania tylko będzie posortować rekordy na aktualnie otwartą strony danych. Na przykład jeśli konfigurujesz kontrolki GridView do obsługują także stronicowania, a następnie, podczas wyświetlania na pierwszej stronie danych, posortuj według nazwy produktu w kolejności malejącej, jej będzie odwrócić kolejność produktów na stronie 1. Jak pokazano na rysunku 18, takie miało tygrysy Carnarvon produktów pierwsze podczas sortowania w odwrotnej kolejności alfabetycznej, co powoduje ignorowanie 71 innych produktów, które pochodzą tygrysy Carnarvon alfabetycznie; tylko te rekordy, na pierwszej stronie zostaną uwzględnione podczas sortowania.


[![Tylko dane wyświetlane na bieżącej stronie jest sortowana](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**Rysunek 18**: tylko dane wyświetlane na bieżącej stronie jest sortowana ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


Sortowanie ma zastosowanie tylko do bieżącej strony dane ponieważ sortowanie jest wykonywane po pobraniu danych z s LOGIKI `GetProductsPaged` metody, a metoda ta zwraca tylko te rekordy, w poszukiwaniu konkretnej strony. Aby zaimplementować sortowanie poprawnie, musimy wyrażenie sortowania, aby przekazać `GetProductsPaged` metody, dzięki czemu dane można sklasyfikować odpowiednio przed zwróceniem określonej strony danych. Zobaczymy, jak to zrobić w naszym samouczku dalej.

## <a name="implementing-custom-paging-and-deleting"></a>Implementowanie niestandardowego stronicowanie i usuwanie

Jeśli użytkownik włączenie funkcji usuwania w GridView, którego dane są stronicowane, przy użyciu niestandardowych metod stronicowania znajdziesz, które podczas usuwania ostatniego rekordu z ostatniej strony widoku GridView znika zamiast odpowiednio zmniejszanie GridView s `PageIndex`. Aby odtworzyć tę usterkę, należy włączyć usuwanie samouczek, który właśnie został dopiero utworzony. Przejdź do ostatniej strony (strona 9), gdzie powinien zostać wyświetlony jeden produkt, ponieważ firma Microsoft stronicowanie 81 produktów, 10 produktów naraz. Usuń ten produkt.

Podczas usuwania ostatniego produktu widoku GridView *powinien* automatyczne przejście do strony ósmego i funkcjonalność wystawiony jest z domyślną stronicowania. Za pomocą niestandardowych stronicowania, jednak po usunięciu ostatniego produktu na ostatniej stronie widoku GridView po prostu znika z ekranu całkowicie. Dokładne przyczyny *Dlaczego* dzieje się to nieco poza zakres tego samouczka, zobacz [usuwanie ostatniego rekordu na ostatniej stronie z GridView za pomocą niestandardowego stronicowania](http://scottonwriting.net/sowblog/posts/7326.aspx) niskiego poziomu szczegółowe informacje dotyczące źródła Ten problem. W podsumowaniu go s z powodu następującej sekwencji czynności, wykonywane przez widoku GridView po kliknięciu przycisku usuwania:

1. Usuń rekord
2. Uzyskaj odpowiednie rekordy do wyświetlenia dla określonego `PageIndex` i `PageSize`
3. Zaznacz, aby upewnić się, że `PageIndex` nie przekracza liczbę stron danych w źródle danych; jeśli go automatycznie dekrementacja GridView s `PageIndex` właściwości
4. Powiązanie odpowiednie strony danych do kontrolki GridView przy użyciu rekordów uzyskany w kroku 2

Ten problem wynika z faktu, że w kroku 2 `PageIndex` używana w przypadku rekordów, aby wyświetlić dane nadal `PageIndex` ostatniej strony, którego jedynym rekord właśnie został usunięty. W związku z tym, w kroku 2 *nie* zwracane są rekordy, ponieważ w tej ostatniej strony danych nie zawiera już żadnych rekordów. Następnie, w kroku 3 widoku GridView zdaje sobie sprawę, że jego `PageIndex` właściwości jest większa niż łączna liczba stron w źródle danych (ponieważ możemy ve usunąć ostatni rekord w ostatniej strony) i w związku z tym zmniejsza jego `PageIndex` właściwości. W kroku 4 próbuje się powiązać danych pobranych w kroku 2; widoku GridView Jednak w kroku 2 nie zwrócono żadnych rekordów, w związku z tym wynikiem pusty GridView. Za pomocą stronicowania domyślnej ten problem t powierzchni, ponieważ w kroku 2 *wszystkich* pobierania rekordów ze źródła danych.

Aby rozwiązać ten problem mamy dwie opcje. Pierwsza to aby utworzyć program obsługi zdarzeń dla GridView s `RowDeleted` program obsługi zdarzeń, która określa liczbę rekordów były wyświetlane na stronie, który właśnie został usunięty. Jeśli było tylko do jednego rekordu, rekord, po prostu usunąć musi zostać ostatni z nich i musimy dekrementacja GridView s `PageIndex`. Oczywiście ma być uruchamiany tylko zaktualizować `PageIndex` Jeśli rzeczywiście powiodła operacji usuwania, które można określić poprzez zapewnienie, że `e.Exception` właściwość `null`.

Ta metoda działa, ponieważ powoduje zaktualizowanie `PageIndex` po kroku 1, ale przed krok 2. W związku z tym w kroku 2, odpowiedni zestaw rekordów jest zwracana. W tym celu należy użyć kodu, jak pokazano poniżej:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

Jeszcze inne obejście polega na utworzeniu program obsługi zdarzeń dla ObjectDataSource s `RowDeleted` zdarzenia i ustawić `AffectedRows` właściwości na wartość 1. Po usunięciu rekordu w kroku 1 (ale przed ponownym podczas pobierania danych w kroku 2), aktualizuje widoku GridView jego `PageIndex` właściwość, jeśli co najmniej jeden wiersz wpłynęła na operację. Jednak `AffectedRows` nie ustawiono właściwości według kontrolki ObjectDataSource i w związku z tym ten krok zostanie pominięty. Jednym ze sposobów, aby ten krok wykonywany jest ręcznie ustawić `AffectedRows` właściwości po pomyślnym ukończeniu operacji usuwania. Można to osiągnąć przy użyciu kodu, jak pokazano poniżej:


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

Kod dla obu tych programów obsługi zdarzeń, można znaleźć w klasie CodeBehind `EfficientPaging.aspx` przykład.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Porównywanie wydajności domyślne i niestandardowe stronicowania

Ponieważ stronicowania niestandardowego pobiera tylko wymagane rekordy, natomiast zwraca domyślne stronicowania *wszystkich* rekordy na każdej stronie, można wyświetlać go s Wyczyść, że stronicowania niestandardowego jest bardziej wydajne niż domyślna stronicowania. Jednak samo jak znacznie bardziej efektywne jest stronicowania niestandardowego? Jakiego rodzaju wzrost wydajności są widoczne, przechodząc z domyślną stronicowania do niestandardowych stronicowania?

Niestety, występują s rozmiar jednego, nie pasuje do wszystkich odpowiedzi w tym miejscu. Przyrost wydajności zależy od wielu czynników, najbardziej znaczącym dwie liczby rekordów są stronicowane za pośrednictwem i obciążenia są umieszczane w kanały bazy danych serwera i komunikacji między serwerem sieci web a serwerem bazy danych. Małe tabele z kilku rekordów kilkudziesięciu różnicę w wydajności może być niewielki. Dla dużych tabel z tysiącami do setek tysięcy wierszy jednak różnicę w wydajności jest istotnym.

Artykuł min, [stronicowania niestandardowe w programie ASP.NET 2.0 przy użyciu programu SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), zawiera pewne testy wydajności I zakończyło się różnice w wydajności między te dwie techniki stronicowania, gdy stronicowanie tabeli bazy danych przy użyciu następującej liczby etapów stwierdzono 50 000 rekordów. W tych testach I zbadać czasu można wykonać zapytania na poziomie serwera SQL (przy użyciu [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) i na stronie ASP.NET za pomocą [s śledzenia programu ASP.NET](https://msdn.microsoft.com/library/y13fw6we.aspx). Należy pamiętać, że te testy były uruchamiane na Moje pole rozwoju z żadnym użytkownikiem aktywne i w związku z tym są unscientific i nie imitują wzorców obciążenia typowe witryny sieci Web. Niezależnie od tego wyniki pokazują względne różnice w czasie wykonywania dla domyślnych i niestandardowych stronicowania, pracując z wystarczająco dużą ilością danych.


|  | **Średni Czas trwania (s)** | **Odczytuje** |
| --- | --- | --- |
| **Domyślne stronicowania SQL Profiler** | 1.411 | 383 |
| **Niestandardowe stronicowania SQL Profiler** | 0.002 | 29 |
| **Domyślne stronicowania danych śledzenia ASP.NET** | 2.379 | *N/D* |
| **Niestandardowe śledzenia ASP.NET stronicowania** | 0.029 | *N/D* |


Jak widać, podczas pobierania określonej strony danych średnio wymagane 354 mniej operacji odczytu i ukończyć w zaledwie ułamku czasu. Na stronie ASP.NET niestandardowe strony był w stanie renderowane w pobliżu 1/100<sup>th</sup> czasu, jaki zajęło podczas korzystania z domyślnej stronicowania. Zobacz [Moje artykułu](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) Aby uzyskać więcej informacji na temat tych wyników, wraz z kodem i bazę danych można pobrać w celu odtworzenia te testy we własnym środowisku.

## <a name="summary"></a>Podsumowanie

Stronicowanie domyślny jest proste wdrożenie po prostu wyboru pole wyboru Włącz stronicowania w danych w sieci Web kontroli s tagu, ale takie uproszczenia pochodzi kosztem wydajności. Za pomocą domyślnego stronicowania, gdy użytkownik zażąda dowolnej strony danych *wszystkich* zwracane są rekordy, nawet jeśli tylko niewielki ułamek z nich mogą być wyświetlane. Aby walczyć z tym zmniejszenie wydajności kontrolki ObjectDataSource oferuje alternatywną stronicowania niestandardowych opcji stronicowania.

Gdy stronicowania niestandardowego poprawia domyślnego stronicowania problemy z wydajnością s przez pobieranie tylko te rekordy, które muszą zostać wyświetlone, jego s więcej wysiłku zaimplementować niestandardowy stronicowania. Po pierwsze zapytanie musi być napisana, który prawidłowo (i efektywnie) uzyskuje dostęp do określony podzbiór żądanych rekordów. Można to zrobić na wiele sposobów; jeden zbadaliśmy w ramach tego samouczka jest użycie programu SQL Server 2005 s nowego `ROW_NUMBER()` wyniki funkcji rangę, a następnie zwracać tylko te wyniki, których klasyfikacji mieści się w określonym zakresie. Ponadto należy dodać środki do określenia całkowita liczba rekordów, które są stronicowane za pośrednictwem. Po utworzeniu tych metod DAL i logiki warstwy Biznesowej, należy również skonfigurować kontrolki ObjectDataSource, dzięki czemu można określić, ile łącznie rekordów są są stronicowane za pośrednictwem i poprawnie można przekazać wartości Rozpocznij indeks wiersza i maksymalna liczba wierszy do LOGIKI.

Implementowanie stronicowania niestandardowe wymagają wielu kroków i nie niemal sprowadza się stronicowania domyślne, stronicowania niestandardowego jest to konieczne, jeśli stronicowanie wystarczająco duże ilości danych. Jak wyniki badania stronicowania pokazały, że, niestandardowe można zmniejszenia sekund poza czas renderowania strony ASP.NET i rozjaśnić obciążenia na serwerze bazy danych przez jedną lub więcej rzędów.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](paging-and-sorting-report-data-vb.md)
> [dalej](sorting-custom-paged-data-vb.md)
