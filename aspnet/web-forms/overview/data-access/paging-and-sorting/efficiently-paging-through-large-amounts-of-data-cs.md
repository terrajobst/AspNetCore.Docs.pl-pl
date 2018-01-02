---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: "Efektywne stronicowania za pośrednictwem dużych ilości danych (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Podczas pracy z dużą ilością danych, jako jego podstawowy pobierania kontroli źródła danych nie nadaje się domyślną opcją stronicowania formantu prezentacji danych..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 5188c7119260e36eb61e9f57cadce29fefdd8016
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="efficiently-paging-through-large-amounts-of-data-c"></a>Efektywne stronicowania za pośrednictwem dużych ilości danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) lub [pobierania plików PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> Domyślna opcja stronicowania danych formantu prezentacji nie nadaje się podczas pracy z dużą ilością danych, jako jego podstawowy kontroli źródła danych pobiera wszystkie rekordy, nawet jeśli jest wyświetlany tylko podzestaw danych. W takiej sytuacji, możemy wyłączyć do niestandardowych stronicowania.


## <a name="introduction"></a>Wprowadzenie

Jak wspomniano w poprzednim samouczek stronicowania można zaimplementować w jeden z dwóch sposobów:

- **Domyślne stronicowania** może być zaimplementowany przez po prostu zaznaczenie opcji Włącz stronicowanie w danych formantu sieci Web s tagów inteligentnych; jednak zawsze, gdy wyświetlania strony danych, pobiera ObjectDataSource *wszystkie* rekordów, nawet jednak tylko ich podzbiór są wyświetlane na stronie
- **Stronicowania niestandardowego** poprawia wydajność domyślna stronicowania przez pobranie tylko te rekordy z bazy danych, konieczne będzie wyświetlany dla określonej strony danych żądanej przez użytkownika; jednak stronicowania niestandardowego wymaga nieco więcej wysiłku do zaimplementowania niż domyślne stronicowania

Ze względu na łatwość implementacji tylko sprawdzanie pole wyboru i której gotowe! domyślne stronicowania jest atrakcyjną opcję. Jego podejścia kolejnych na pobranie wszystkich rekordów, jednak ułatwia wybór nieprawdopodobny podczas stronicowania za pośrednictwem wystarczająco duże ilości danych lub dla witryn o wielu użytkowników równocześnie. W takiej sytuacji możemy wyłączyć do niestandardowych stronicowania w celu zapewnienia elastyczny system.

Wyzwanie stronicowania niestandardowego jest możliwość Napisz zapytanie, które zwraca dokładny zestaw rekordów niezbędnych dla określonej strony danych. Na szczęście usługa Microsoft SQL Server 2005 zawiera new — słowo kluczowe wyników klasyfikacji, co umożliwia firmie Microsoft w celu Napisz zapytanie, które skutecznie można pobrać prawidłowego podzestaw rekordów. W tym samouczku będziesz przedstawiono sposób użycia to słowo kluczowe new programu SQL Server 2005 do implementowania stronicowania niestandardowego w kontrolce GridView. Gdy interfejs użytkownika stronicowania niestandardowego jest identyczna jak domyślne stronicowanie, wykonywanie krok po kroku z jednej strony do następnego przy użyciu stronicowania niestandardowego może mieć wiele rzędów szybciej niż domyślne stronicowania.

> [!NOTE]
> Bardziej wydajne dokładne, wystawiane przez stronicowania niestandardowego zależy od całkowita liczba rekordów jest stronicowana za pośrednictwem i obciążenia są umieszczone na serwerze bazy danych. Na końcu tego samouczka przyjrzymy niektórych nierównej metryki pokazują zalet wydajności uzyskiwaną stronicowania niestandardowego.


## <a name="step-1-understanding-the-custom-paging-process"></a>Krok 1: Zrozumienie niestandardowy proces stronicowania

Gdy stronicowanie za pośrednictwem danych, dokładne rekordów wyświetlanych na stronie zależą strony danych żądanej i liczba rekordów wyświetlanych na stronie. Na przykład załóżmy, że możemy przeglądania 81 produktów, wyświetlanie 10 produktów na stronie. Podczas wyświetlania pierwszej strony, d chcemy produktów od 1 do 10; Podczas wyświetlania na drugiej stronie możemy d zainteresować produktów 11 do 20 i tak dalej.

Istnieją trzy zmienne, które wymuszają rekordy muszą zostać pobrane i jak mają być renderowane interfejsu stronicowania:

- **Indeks wiersza początkowy** indeks pierwszego wiersza na stronie danych do wyświetlenia; może to indeks jest obliczana przez pomnożenie indeksu strony przez rekordów wyświetlanych na każdej stronie i dodawania co. Na przykład jeśli stronicowanie za pośrednictwem rekordów 10 jednocześnie, na pierwszej stronie (której indeks strony wynosi 0), indeks wiersza Start wynosi 0 \* 10 + 1 lub 1; dla drugiej strony (indeks strony, którego wartość wynosi 1) jest indeks wiersza Start 1 \* 10 + 1 , lub 11.
- **Maksymalna liczba wierszy** maksymalną liczbę rekordów wyświetlanych na każdej stronie. Ta zmienna jest określana jako maksymalna liczba wierszy, ponieważ dla ostatniej strony może być mniejszą liczbę rekordów zwróconych niż rozmiar strony. Na przykład stronicowanie rekordy 81 produktów 10 na każdej stronie, dziewiąty i ostatnia strona ma tylko jeden rekord. Jednak Brak strony zostaną wyświetlone więcej rekordów niż wartość maksymalna liczba wierszy.
- **Całkowita liczba rekordów** całkowita liczba rekordów jest stronicowana za pośrednictwem. Gdy t ta zmienna nie jest potrzebne do ustalenia, które rekordy do pobrania dla danej strony, wymagane przez interfejs stronicowania. Na przykład w przypadku produktów 81 trwa stronicowanej za pośrednictwem interfejsu stronicowania wie do wyświetlenia dziewięć cyfr strony w interfejsie użytkownika stronicowania.

Z stronicowania domyślny indeks wiersza Start jest obliczana jako iloczyn indeksu strony i rozmiar strony plus, maksymalna liczba wierszy jest po prostu rozmiar strony. Ponieważ domyślne stronicowania pobiera wszystkie rekordy z bazy danych podczas renderowania dowolnej strony indeksu dla każdego wiersza danych jest znany, dzięki czemu przenoszenie do wiersza Start indeks wiersza trivial zadań. Ponadto jest dostępny, ponieważ łączna liczba rekordów s po prostu liczbę rekordów w elemencie DataTable (lub obiekt jest używany do przechowywania wyników bazy danych).

Podana zmienne Start indeks wiersza i maksymalna liczba wierszy, implementacja niestandardowa stronicowania tylko musi zwracać dokładne podzestaw uruchamianie w indeksie wiersza Start i maksymalnie wierszy maksymalną liczbę rekordów po tym. Stronicowania niestandardowego zapewnia wyzwanie z dwóch powodów:

- Firma Microsoft musi mieć możliwość wydajnie skojarzyć indeks wiersza z każdego wiersza w całej danych jest stronicowanej za pośrednictwem, dzięki czemu możemy rozpocząć zwracanie rekordów w określonym indeksie wiersza Start
- Należy podać całkowita liczba rekordów jest stronicowana za pośrednictwem

W poniższych dwóch krokach zajmiemy skrypt SQL niezbędne do odpowiadania na te dwa problemy. Oprócz skryptu SQL potrzebujemy również implementować metody DAL i logiki warstwy Biznesowej.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Krok 2: Całkowita liczba rekordów jest stronicowana za pośrednictwem zwracanie

Przed omówione jak uzyskać dokładne podzestaw rekordów dla strony wyświetlany umożliwiają Pierwsze spojrzenie na sposób zwracania całkowita liczba rekordów jest stronicowana za pośrednictwem s. Te informacje są potrzebne, aby poprawnie skonfigurować interfejs użytkownika stronicowania. Całkowita liczba rekordów zwróconych przez zapytanie SQL w szczególności można uzyskać za pomocą [ `COUNT` funkcję agregacji](https://msdn.microsoft.com/en-US/library/ms175997.aspx). Na przykład, aby określić całkowita liczba rekordów w `Products` tabeli, możemy użyć następujące zapytanie:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Umożliwiają dodawanie metody do naszych DAL, która zwraca informację s. W szczególności, utworzymy warstwę DAL metodę o nazwie `TotalNumberOfProducts()` , który jest wykonywany `SELECT` instrukcji przedstawionych powyżej.

Uruchamianie przez otwarcie `Northwind.xsd` wpisane pliku zestawu danych w `App_Code/DAL` folderu. Następnie kliknij prawym przyciskiem myszy `ProductsTableAdapter` w Projektancie i wybierz polecenie Dodaj zapytanie. Jako firma Microsoft kolejnych w poprzedniej samouczki, pozwoli to firmie Microsoft w celu dodania nowej metody z warstwą dal, gdy została wywołana, będą wykonywane określonego instrukcję SQL lub procedurę składowaną. Podobnie jak w przypadku naszego metod TableAdapter w poprzednim samouczki, dla tego zdecydować się na użycie instrukcji SQL ad hoc.


![Używanie instrukcji SQL Ad Hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Rysunek 1**: Użyj instrukcji SQL Ad Hoc


Na następnym ekranie możemy określić jakiego rodzaju zapytanie w celu utworzenia. Ponieważ to zapytanie spowoduje zwrócenie pojedynczego, skalarne wartość całkowita liczba rekordów w `Products` wybierz tabelę `SELECT` zwraca opcji wartość wystąpieniu.


![Skonfiguruj zapytanie do użycia w instrukcji SELECT, która zwraca pojedynczą wartość](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Rysunek 2**: Skonfiguruj zapytanie do użycia w instrukcji SELECT, która zwraca pojedynczą wartość


Po wskazujący typ zapytanie do użycia, możemy obok określ kwerendę.


![Użyj wybierz COUNT(*) z kwerendy produktów](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Rysunek 3**: Użyj wybierz liczby (\*) FROM zapytania produktów


Na koniec należy określić nazwę metody. S wyżej wymienione, pozwalają używać `TotalNumberOfProducts`.


![Nazwa TotalNumberOfProducts DAL — metoda](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Rysunek 4**: Nazwa TotalNumberOfProducts DAL — metoda


Po kliknięciu przycisku Zakończ, Kreator doda `TotalNumberOfProducts` metody z warstwą dal. Skalarne metody przekazujących w warstwy DAL zwracają typy dopuszczające wartości null, w przypadku, gdy wynik zapytania SQL `NULL`. Nasze `COUNT` zapytania, jednak zawsze zwróci niż`NULL` wartości; niezależnie od tego, metoda DAL zwraca całkowitą wartość null.

Oprócz metody DAL potrzebujemy są również metody w logiki warstwy Biznesowej. Otwórz `ProductsBLL` klasy plik i dodać `TotalNumberOfProducts` metodę, która po prostu wywołuje w dół do DAL s `TotalNumberOfProducts` metody:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

DAL s `TotalNumberOfProducts` metoda zwraca całkowitą wartość null, ale firma Microsoft kolejnych utworzone `ProductsBLL` klasy s `TotalNumberOfProducts` metodę, tak aby zwraca standardowy liczbę całkowitą. W związku z tym musimy mieć `ProductsBLL` klasy s `TotalNumberOfProducts` metoda zwraca wartość części całkowitej wartości null zwracane przez warstwę DAL s `TotalNumberOfProducts` metody. Wywołanie `GetValueOrDefault()` zwraca wartość liczby całkowitej wartości null, jeśli istnieje; w przypadku wartości null całkowitą `null`, jednak zwraca wartość liczby całkowitej domyślna to 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Krok 3: Zwracanie dokładne podzestaw rekordów

Nasze następne zadanie to można utworzyć metody w DAL i logiki warstwy Biznesowej, które akceptują indeks wiersza Start i maksymalna liczba wierszy zmiennych wymienione powyżej i zwraca odpowiednie rekordy. Zanim przejdziemy, umożliwiają s Pierwsze spojrzenie na uruchomienie skryptu SQL. Żądania skierowane do nas jest, że firma musi mieć możliwość przydzielić indeksu dla każdego wiersza w całej wyniki są stronicowanej za pośrednictwem tak, aby firma Microsoft może zwracać tylko te rekordy, uruchamianie w indeksie wiersza Start (i do rekordów maksymalnej liczby rekordów).

To nie jest trudne, jeśli istnieje już kolumna w tabeli bazy danych, która służy jako indeks wiersza. Na pierwszy rzut oka może podejrzewamy, że `Products` tabeli s `ProductID` pola będą wystarczające jako pierwszy produkt ma `ProductID` 1, 2, drugi i tak dalej. Jednak usunięcie produktu pozostawia przerwy w sekwencji, rozpoczynającą tej metody.

Istnieją dwie metody Ogólne umożliwia wydajne skojarzyć indeks wiersza z danymi do przeglądania, umożliwiając w ten sposób dokładne podzestaw rekordów do pobrania:

- **Za pomocą programu SQL Server 2005 s `ROW_NUMBER()` — słowo kluczowe** jesteś nowym użytkownikiem programu SQL Server 2005, `ROW_NUMBER()` — słowo kluczowe kojarzy klasyfikacji z każdego zwróconego rekordu według kolejności. Ta klasyfikacja może służyć jako indeks wiersza dla każdego wiersza.
- **Za pomocą zmiennej tabeli i `SET ROWCOUNT`**  s programu SQL Server [ `SET ROWCOUNT` instrukcji](https://msdn.microsoft.com/en-us/library/ms188774.aspx) można określić liczbę rekordów całkowita zapytania ma być przetwarzana przed zakończeniem; [tabeli zmienne](http://www.sqlteam.com/item.asp?ItemID=9454) są zmienne lokalne T-SQL, które mogą zawierać dane tabelaryczne akin do [tabel tymczasowych](http://www.sqlteam.com/item.asp?ItemID=2029). Ta metoda działa równie dobrze z Microsoft SQL Server 2005 i SQL Server 2000 (natomiast `ROW_NUMBER()` podejście działa tylko w przypadku programu SQL Server 2005).  
  
 W tym miejscu będzie utworzyć zmienną tabeli, która ma `IDENTITY` kolumny i kolumny kluczy podstawowych w tabeli jest trwa stronicowanej którego dane za pośrednictwem. Następnie utworzyć zrzutu zawartość tabeli, którego dane jest trwa stronicowanej za pośrednictwem do zmiennej tabeli, w tym samym kojarzenie indeks wiersza sekwencyjnych (za pośrednictwem `IDENTITY` kolumny) dla każdego rekordu w tabeli. Gdy zmienna tabeli zostały wypełnione, `SELECT` instrukcji w zmiennej tabeli połączony z tabeli podstawowej, aby można było wykonać Aby wysunąć określonych rekordów. `SET ROWCOUNT` Używana jest instrukcja inteligentnie ograniczyć liczbę rekordów, które należy można utworzyć zrzutu w zmiennej tabeli.  
  
 Takie zwiększenie efektywności podejście s jest oparta na numer strony żądanej, jako `SET ROWCOUNT` wartość jest przypisywana wartość Start indeks wiersza oraz maksymalna liczba wierszy. Jeśli stronicowanie za pośrednictwem strony o numerach niski, takich jak pierwszy kilka stron danych ta metoda jest bardzo wydajny. Jednak wskazuje domyślną stronicowania przypominającej wydajności podczas pobierania strony blisko końca.

W tym samouczku implementuje niestandardowych za pomocą stronicowania `ROW_NUMBER()` — słowo kluczowe. Aby uzyskać więcej informacji na temat używania zmiennej tabeli i `SET ROWCOUNT` technika, zobacz [A więcej efektywną metodą stronicowania za pośrednictwem dużych zestawów wyników](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

`ROW_NUMBER()` — Słowo kluczowe skojarzone klasyfikacji z każdego rekordu zwróconego w kolejności określonej przy użyciu następującej składni:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()`Zwraca wartość liczbową określa rangę dla każdego rekordu w odniesieniu do wskazanej kolejności. Na przykład aby wyświetlić pozycję dla każdego produktu, uporządkowanych od najbardziej kosztowne o najmniejszej można Stosujemy następujące zapytanie:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

Rysunek 5. Pokazuje to zapytanie wyniki s uruchamianych w oknie zapytania w programie Visual Studio. Należy pamiętać, że te produkty są uporządkowane według cen wraz z pozycji ceny dla każdego wiersza.


![Ranga cen jest dołączane do każdego rekordu zwrócone](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Rysunek 5**: rangę cen jest dołączane do każdego rekordu zwrócone


> [!NOTE]
> `ROW_NUMBER()`jest tylko jeden z wielu nowych funkcji klasyfikacji dostępnych w programie SQL Server 2005. Bardziej szczegółowe omówienie `ROW_NUMBER()`, oraz inne funkcje klasyfikacji, przeczytaj [zwracanie wyniki związane z programu Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).


Gdy klasyfikacja wyniki według określonego `ORDER BY` kolumny w `OVER` klauzuli (`UnitPrice`, w powyższym przykładzie), programu SQL Server musi sortowania wyników. To jest szybkie działanie, jeśli istnieje indeks klastrowany kolumn na liście wyników jest porządkowana, lub jeśli istnieje pokryciem indeksu, ale może być bardziej kosztowne inaczej. Aby zwiększyć wydajność kwerend wystarczająco duże, należy rozważyć dodanie indeks nieklastrowany za pomocą której wyniki są uporządkowane według kolumny. Zobacz [funkcji klasyfikacji i wydajności w programie SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) dla bardziej szczegółowy widok zagadnienia dotyczące wydajności.

Informacje dotyczące klasyfikacji zwracane przez `ROW_NUMBER()` nie można użyć bezpośrednio w `WHERE` klauzuli. Jednak tabeli pochodnej może służyć do zwrócenia `ROW_NUMBER()` wynik, który następnie może występować w `WHERE` klauzuli. Na przykład poniższe zapytanie używa tabeli pochodnej mają być zwracane ProductName i UnitPrice kolumny, wraz z `ROW_NUMBER()` wyników, a następnie używa `WHERE` klauzuli, która zwraca tylko te produkty której pozycję cen to od 11 do 20:


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Rozszerzanie nieco więcej koncepcji, firma Microsoft może korzystać z tej metody można pobrać określonej strony danych podanych odpowiednie wartości Start indeks wiersza i maksymalna liczba wierszy:


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Jak zostanie wyświetlone później w tym samouczku  *`StartRowIndex`*  dostarczonych przez element ObjectDataSource jest indeksowana zaczynając od zera, podczas gdy `ROW_NUMBER()` wartość zwrócona przez program SQL Server 2005 jest indeksowana, zaczynając od 1. W związku z tym `WHERE` klauzula zwraca te rekordy, gdzie `PriceRank` jest większa niż  *`StartRowIndex`*  i mniejsza niż lub równa  *`StartRowIndex`*   +  *`MaximumRows`*.


Teraz tego możemy kolejnych omówiony sposób `ROW_NUMBER()` mogą być używane do pobierania określonej strony danych podanych wartości Start indeks wiersza i maksymalna liczba wierszy, teraz należy wdrożyć logikę tej metody w DAL i logiki warstwy Biznesowej.

Podczas tworzenia to zapytanie, należy zdecydować, kolejność za pomocą którego wyników zostanie wyznaczona ranga; Let s posortować produkty według nazwy w kolejności alfabetycznej. Oznacza to, że z implementacją stronicowania niestandardowego, w tym samouczku firma Microsoft nie będzie można utworzyć niestandardowy raport stronicowanych niż również można sortować. W następnym samouczku, firma Microsoft będzie widoczny jak takie funkcje można podać.

W poprzedniej sekcji utworzyliśmy metody DAL jako instrukcję SQL ad hoc. Niestety, analizator T-SQL w programie Visual Studio używane przez t kreatora TableAdapter, takich jak `OVER` składnia wykorzystywana przez `ROW_NUMBER()` funkcji. W związku z tym musi utworzymy tej metody DAL jako procedury składowanej. Wybierz z menu Widok (lub trafień Ctrl + Alt + S) w Eksploratorze serwera i rozwiń `NORTHWND.MDF` węzła. Aby dodać nową procedurę składowaną, kliknij prawym przyciskiem myszy w węźle procedur składowanych i wybierz polecenie Dodaj nowe procedury składowanej (patrz rysunek 6).


![Dodaj nową procedurę składowaną stronicowanie za pośrednictwem produktów](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Rysunek 6**: Dodaj nową procedurę składowaną stronicowanie za pośrednictwem produktów


Tę procedurę składowaną powinna obsługiwać dwóch parametrów wejściowych całkowitą — `@startRowIndex` i `@maximumRows` i użyj `ROW_NUMBER()` funkcja uporządkowanych według `ProductName` pola zwracanie tylko tych wierszy, większy niż określony `@startRowIndex` i mniejsza niż lub równa `@startRowIndex`  +  `@maximumRow` s. Wprowadź następujący skrypt do nowej procedury składowanej, a następnie kliknij ikonę Zapisz, aby dodać procedury składowanej do bazy danych.


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Po utworzeniu procedury składowanej, Poświęć chwilę, aby przetestować go. Kliknij prawym przyciskiem myszy `GetProductsPaged` procedury składowanej nazwy w Eksploratorze serwera i wybierz opcję Execute. Visual Studio wyświetlany jest monit dla parametrów wejściowych `@startRowIndex` i `@maximumRow` s (patrz rysunek 7). Spróbuj różne wartości, a następnie przejrzyj wyniki.


![Wprowadź wartość dla @startRowIndex i @maximumRows parametrów](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

**Rysunek 7**: wprowadź wartość dla @startRowIndex i @maximumRows parametrów


Po wybranie tych wprowadzanie wartości parametrów, okno dane wyjściowe będą pokazywały wyniki. Rysunek nr 8 przedstawia wyniki podczas przekazywania na 10 dla obu `@startRowIndex` i `@maximumRows` parametrów.


[![Rejestruje że pojawią się w drugiej strony danych są zwracane.](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Rysunek 8**: rekordy że pojawią się w drugiej strony dane są zwracane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))


Z tym przechowywane procedury tworzenia, możemy re gotowy do utworzenia `ProductsTableAdapter` metody. Otwórz `Northwind.xsd` wpisane zestawu danych, kliknij prawym przyciskiem myszy w `ProductsTableAdapter`, a następnie wybierz opcję Dodaj zapytanie. Zamiast tworzenia zapytania przy użyciu instrukcji SQL ad-hoc, należy utworzyć za pomocą istniejącą procedurę składowaną.


![Create — metoda DAL przy użyciu istniejącą procedurę składowaną](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Rysunek 9**: Utwórz metody DAL przy użyciu istniejącą procedurę składowaną


Następnie możemy wyświetlony monit o wybranie procedury składowanej do wywołania. Wybierz `GetProductsPaged` przechowywane procedury z listy rozwijanej.


![Wybierz GetProductsPaged przechowywane procedury z listy rozwijanej](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Na rysunku nr 10**: Wybierz GetProductsPaged przechowywane procedury z listy rozwijanej


Następnym ekranie następnie prosi jakiego rodzaju dane zwracane przez procedurę składowaną: dane tabelaryczne, pojedynczą wartością lub brak wartości. Ponieważ `GetProductsPaged` procedurę składowaną można przywrócić wiele rekordów, wskazują, że zwraca dane tabelaryczne.


![Wskazuje, że procedura składowana ma zwracać dane tabelaryczne](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Rysunek 11**: wskazuje, że procedura składowana ma zwracać dane tabelaryczne


Wskazuje koniec nazwy metody, które ma zostać utworzona. Zgodnie z naszymi samouczkami, poprzednie, przejdź dalej i Utwórz metody za pomocą obu wypełnienia DataTable i zwracają DataTable. Pierwsza metoda nazwa `FillPaged` , a drugi `GetProductsPaged`.


![Nazwa metody FillPaged i GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Rysunek 12**: Nazwa FillPaged metod i GetProductsPaged


Ponadto można utworzyć metody warstwy DAL do zwracania określonej strony produktów, również potrzebujemy umożliwiają korzystanie z tych funkcji w logiki warstwy Biznesowej. Jak metoda DAL s logiki warstwy Biznesowej metody GetProductsPaged dwóch wejść całkowitą Start indeks wiersza i maksymalna liczba wierszy, musisz zaakceptować i musi zwracać tylko te rekordy, które należą do zakresu. Utwórz metodę logiki warstwy Biznesowej w klasie ProductsBLL, że tylko wywołania w dół do s DAL metody GetProductsPaged w następujący sposób:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Można użyć dowolnej nazwy parametrów wejściowych metody s logiki warstwy Biznesowej, ale, jak firma Microsoft będzie wkrótce, wybrania `startRowIndex` i `maximumRows` oszczędza nam dodatkowy bit pracy podczas konfigurowania ObjectDataSource przy użyciu tej metody.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Krok 4: Konfigurowanie ObjectDataSource do użycia stronicowania niestandardowego

Za pomocą metod logiki warstwy Biznesowej i warstwy DAL do uzyskiwania dostępu do konkretnego podzestawu rekordów pełną re gotowe, aby utworzyć element GridView sterować tej strony za pomocą jego rekordy przy użyciu stronicowania niestandardowego. Uruchamianie przez otwarcie `EfficientPaging.aspx` strony `PagingAndSorting` folderu, na stronie Dodaj element GridView i skonfigurować go do używania nowej kontrolki ObjectDataSource. W naszym ostatnich samouczki, często było ObjectDataSource skonfigurowana do używania `ProductsBLL` klasy s `GetProducts` metody. Teraz, jednak chcemy użyć `GetProductsPaged` — metoda, ponieważ `GetProducts` metoda zwraca *wszystkie* produktów w bazie danych należy `GetProductsPaged` zwraca konkretnego podzestawu rekordów.


![Skonfiguruj element ObjectDataSource przy użyciu metody GetProductsPaged klasy ProductsBLL s](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Rysunek 13**: Konfigurowanie ObjectDataSource przy użyciu metody GetProductsPaged klasy ProductsBLL s


Od możemy re tworzenie tylko do odczytu widoku GridView Poświęć chwilę, aby ustawić metodę listy rozwijanej w INSERT, UPDATE i usuwanie kart na (Brak).

Następnie Kreator ObjectDataSource nam monituje o podanie źródła `GetProductsPaged` metody s `startRowIndex` i `maximumRows` wprowadzanie wartości parametrów. Te parametry wejściowe faktycznie zostanie ustawiona w widoku GridView automatycznie, więc po prostu pozostaw zestaw źródła None i kliknij przycisk Zakończ.


![Pozostaw źródeł parametru wejściowego None](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Rysunek 14**: pozostaw źródeł parametru wejściowego None


Po zakończeniu pracy Kreatora ObjectDataSource widoku GridView będzie zawierać elementu BoundField lub CheckBoxField dla każdego pola danych produktu. Możesz dostosować wygląd widoku GridView s zgodnie z własnymi potrzebami. I Zapisz zgłoszono mają być wyświetlane tylko `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`, i `UnitPrice` BoundFields. Ponadto należy skonfigurować do obsługi stronicowania, zaznaczając pole wyboru Włącz stronicowanie w jego tagów inteligentnych widoku GridView. Po wprowadzeniu tych zmian znaczników deklaratywne GridView i ObjectDataSource powinien wyglądać podobny do następującego:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

W przypadku odwiedzenia strony za pośrednictwem przeglądarki, jednak widoku GridView jest którego ma zostać odnaleziona.


![Widoku GridView jest niewidoczne](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Rysunek 15**: widoku GridView jest niewidoczne


Widoku GridView jest Brak, ponieważ element ObjectDataSource aktualnie używa 0 jako wartości dla obu `GetProductsPaged` `startRowIndex` i `maximumRows` parametrów wejściowych. W związku z tym żadne rekordy nie zwraca wynikowy zapytania SQL i w związku z tym nie jest wyświetlany widoku GridView.

Aby rozwiązać ten problem, należy skonfigurować element ObjectDataSource do użycia stronicowania niestandardowego. Można to zrobić w poniższych krokach:

1. **Ustaw element ObjectDataSource s `EnablePaging` właściwości `true`**  wskazuje element ObjectDataSource, który musi upłynąć do `SelectMethod` dwóch dodatkowych parametrów: jedna, aby określić indeks wiersza Start ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) i określ maksymalna liczba wierszy ([`MaximumRowsParameterName`](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Ustaw ObjectDataSource s `StartRowIndexParameterName` i `MaximumRowsParameterName` odpowiednio do właściwości** `StartRowIndexParameterName` i `MaximumRowsParameterName` właściwości wskazują nazwy parametrów wejściowych przekazany `SelectMethod` celach stronicowania niestandardowego . Domyślnie te nazwy parametru są `startIndexRow` i `maximumRows`, czyli Dlaczego, podczas tworzenia `GetProductsPaged` metody w logiki warstwy Biznesowej, można użyć tych wartości parametrów wejściowych. Jeśli zostanie wybrany różne nazwy parametrów dla s logiki warstwy Biznesowej `GetProductsPaged` metody, takie jak `startIndex` i `maxRows`dla przykładzie trzeba ustawić ObjectDataSource s `StartRowIndexParameterName` i `MaximumRowsParameterName` właściwości odpowiednio (na przykład startIndex dla `StartRowIndexParameterName` i maksymalna liczba wierszy do `MaximumRowsParameterName`).
3. **Ustaw element ObjectDataSource s [ `SelectCountMethod` właściwości](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) na nazwę metody, która zwraca łączna liczba z rekordów jest stronicowanej za pośrednictwem (`TotalNumberOfProducts`)** odwołania, który `ProductsBLL` klasy s `TotalNumberOfProducts`metoda zwraca wartość całkowita liczba rekordów jest stronicowana przy użyciu metody DAL, która wykonuje `SELECT COUNT(*) FROM Products` zapytania. Te informacje są potrzebne przez element ObjectDataSource, aby można było poprawnie renderowania interfejsu stronicowania.
4. **Usuń `startRowIndex` i `maximumRows` `<asp:Parameter>` elementy z s ObjectDataSource deklaratywne znaczników** podczas konfigurowania ObjectDataSource za pomocą kreatora, Visual Studio automatycznie dodane dwa `<asp:Parameter>` elementy dla `GetProductsPaged` metody s parametrów wejściowych. Przez ustawienie `EnablePaging` do `true`, parametry te będą przekazywane automatycznie; jeśli znajdują się również w składni deklaratywnej, ObjectDataSource spróbuje przekazać *cztery* parametry `GetProductsPaged` — metoda i dwa parametry `TotalNumberOfProducts` metody. Jeśli użytkownik zapomni usunąć te `<asp:Parameter>` elementów, podczas odwiedzania strony za pośrednictwem przeglądarki, zostanie wyświetlony komunikat o błędzie, takich jak: *element ObjectDataSource "ObjectDataSource1" nie można znaleźć nieuniwersalnej metody TotalNumberOfProducts, która ma Parametry: startRowIndex, maximumRows*.

Po wprowadzeniu tych zmian, składni deklaratywnej s ObjectDataSource powinna wyglądać następująco:


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Należy pamiętać, że `EnablePaging` i `SelectCountMethod` zostały ustawione właściwości i `<asp:Parameter>` elementy zostały usunięte. 16 rysunku przedstawiono zrzut ekranu: okno właściwości po dokonaniu zmiany.


![Aby użyć niestandardowej stronicowania, skonfiguruj kontrolki ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Rysunek 16**: Aby użyć niestandardowej stronicowania, skonfiguruj kontrolki ObjectDataSource


Po wprowadzeniu tych zmian, odwiedź stronę tej strony za pośrednictwem przeglądarki. Powinny pojawić się 10 produktów na liście, w kolejności alfabetycznej. Poświęć chwilę, aby przetwarzać dane o jedną stronę w czasie. Gdy nie ma różnic visual z perspektywy użytkownika końcowego s między stronicowania domyślne i niestandardowe stronicowania, niestandardowe stronicowania wydajniej strony z dużą ilością danych zgodnie z jego pobiera tylko te rekordy, które muszą być wyświetlany dla danej strony.


[![Dane, zamówione przez produkt s nazwa jest stronicowania niestandardowego stronicowanej przy użyciu](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Rysunek 17**: dane, zamówione przez produkt s nazwa jest stronicowania niestandardowego stronicowanej przy użyciu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))


> [!NOTE]
> Z stronicowania niestandardowego strony liczba wartość zwrócona przez element ObjectDataSource s `SelectCountMethod` znajduje się w stanie widoku GridView s. Inne zmienne GridView `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` kolekcji i tak dalej są przechowywane w *kontrolować stan*, który jest trwały, niezależnie od wartości GridView s `EnableViewState` Właściwość. Ponieważ `PageCount` wartość jest trwale przechowywana w ogłaszania zwrotnego przy użyciu stan widoku, korzystając z interfejsu stronicowania, który zawiera łącze umożliwiające przejście do ostatniej strony, konieczne jest włączenie stan widoku GridView s. (Jeśli interfejs użytkownika stronicowania ostatni nie ma bezpośredniego łącza strony, następnie należy wyłączyć stan widoku).


Kliknij link do ostatniej strony powoduje odświeżenie strony i powoduje, że można zaktualizować widoku GridView jego `PageIndex` właściwości. Po kliknięciu link do ostatniej strony widoku GridView przypisuje jej `PageIndex` właściwości na wartość jeden mniejsza od jego `PageCount` właściwości. Widok stanu wyłączone `PageCount` wartości zostaną utracone na ogłaszania zwrotnego i `PageIndex` zamiast tego przypisuje się wartość maksymalna liczba całkowita. Następnie widoku GridView spróbuje określić początkowy indeks wiersza przez pomnożenie `PageSize` i `PageCount` właściwości. Powoduje to `OverflowException` ponieważ produktu przekracza rozmiar maksymalny dozwolony liczby całkowitej.

## <a name="implement-custom-paging-and-sorting"></a>Implementowanie niestandardowych stronicowania i sortowania

Nasze bieżąca implementacja stronicowania niestandardowego wymaga kolejności, za pomocą którego dane są stronicowanej za pośrednictwem statycznie podczas tworzenia `GetProductsPaged` procedury składowanej. Jednak może mieć zanotowaną czy tagów inteligentnych s GridView zawiera wyboru Włącz sortowanie oprócz opcji Włącz stronicowanie. Niestety Dodawanie obsługi sortowania do widoku GridView z naszych bieżąca implementacja stronicowania niestandardowego tylko będzie sortowanie rekordów na stronie aktualnie wyświetlanych danych. Na przykład jeśli skonfigurujesz GridView również obsługiwać stronicowania i następnie podczas przeglądania danych, na pierwszej stronie sortowania według nazwy produktu w kolejności malejącej, go będzie odwrócić kolejność produktów na stronie 1. Jak pokazano na rysunku 18, takie tygrysy Carnarvon po wyświetlany jako pierwszy produktu w odwrotnej kolejności alfabetycznej, co powoduje ignorowanie 71 inne produkty, które pochodzą od tygrysy Carnarvon alfabetycznie; tylko te rekordy na pierwszej stronie są wliczane sortowania.


[![Jest sortowany tylko dane wyświetlane na bieżącej stronie](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Rysunek 18**: tylko dane wyświetlane na bieżącej stronie jest sortowana ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))


Sortowanie dotyczy tylko do bieżącej strony danych, ponieważ sortowanie jest wykonywana po pobraniu danych z s logiki warstwy Biznesowej `GetProductsPaged` — metoda i metoda ta zwraca tylko tych rekordów dla określonej strony. Aby zaimplementować sortowanie prawidłowo, należy przekazać wyrażenie sortowania `GetProductsPaged` metody, dzięki czemu dane można odpowiednio uszeregowane przed zwróceniem określonej strony danych. Będzie przedstawiono, jak to zrobić w naszym samouczku dalej.

## <a name="implementing-custom-paging-and-deleting"></a>Implementowanie niestandardowego stronicowania i usuwanie

Jeśli użytkownik włączenie funkcji usuwania w widoku GridView, którego dane są stronicowanej przy użyciu niestandardowych technik stronicowania, można zauważyć, które podczas usuwania ostatniego rekordu z ostatniej strony, widoku GridView zniknie zamiast odpowiednio zmniejszanie GridView s `PageIndex`. Aby odtworzyć ten błąd, Włącz usuwanie samouczek, które właśnie właśnie utworzyliśmy. Przejdź do ostatniej strony (strona 9), gdzie powinien zostać wyświetlony jeden produkt od nas są stronicowania za pośrednictwem 81 produktów, 10 produktów w czasie. Usuń ten produkt.

Podczas usuwania ostatniego produktu widoku GridView *powinien* automatyczne przejście do strony ósmego i takich funkcji wystawiony jest z domyślną stronicowania. Z stronicowania niestandardowego, jednak po usunięciu ostatniego produktu na ostatniej stronie widoku GridView po prostu zniknie z ekranu całkowicie. Dokładne Przyczyna *Dlaczego* dzieje się to nieco wykracza poza zakres tego samouczka; zobacz [usunięcie ostatniego rekordu na ostatniej stronie z Element GridView z właściwością stronicowania niestandardowego](http://scottonwriting.net/sowblog/posts/7326.aspx) dla niskiego poziomu szczegóły dotyczące źródła Ten problem. W podsumowaniu go s z powodu następująca sekwencja kroków, które są wykonywane w widoku GridView po kliknięciu przycisku Usuń:

1. Usuń rekord
2. Pobierz odpowiednie rekordy do wyświetlenia dla określonego `PageIndex` i`PageSize`
3. Sprawdź, upewnij się, że `PageIndex` nie przekracza liczbę stron danych w źródle danych; jeśli ją automatycznie zmniejszyć GridView s `PageIndex` właściwości
4. Powiązać odpowiedniej strony danych widoku GridView przy użyciu rekordów uzyskanym w kroku 2

Problem wynika z faktu, że w kroku 2 `PageIndex` używany, gdy przechwytywanie rekordów do wyświetlenia jest nadal `PageIndex` ostatniej strony, którego jedynym rekord właśnie został usunięty. W związku z tym w kroku 2 *nie* rekordy są zwracane, ponieważ w tej ostatniej strony danych nie zawiera żadnych rekordów. Następnie, w kroku 3 widoku GridView realizuje który jego `PageIndex` właściwości jest większa niż całkowita liczba stron w źródle danych (od nas kolejnych usunąć ostatni rekord na ostatniej stronie) i w związku z tym zmniejsza jego `PageIndex` właściwości. W kroku 4 próbuje się powiązać dane pobrane w kroku 2; widoku GridView Jednak w kroku 2 żadne rekordy nie zostały zwrócone, w związku z tym powodujące pusty element GridView. Z domyślnego stronicowania, ten problem t surface, ponieważ w kroku 2 *wszystkie* rekordy są pobierane ze źródła danych.

Aby rozwiązać ten problem, firma Microsoft są dostępne dwie opcje. Pierwsza to można utworzyć programu obsługi zdarzeń dla widoku GridView s `RowDeleted` obsługi zdarzeń, który określa liczbę rekordów były wyświetlane na stronie, który właśnie został usunięty. Jeśli było tylko jeden rekord, rekord tylko usunięte musi być ostatnią i musimy dekrementacji s widoku GridView `PageIndex`. Oczywiście chcemy tylko zaktualizować `PageIndex` Jeśli operacja usuwania rzeczywiście powiodła się, które można określić za zapewnienie, że `e.Exception` jest właściwość `null`.

Ta metoda działa, ponieważ aktualizuje `PageIndex` po kroku 1, ale przed krok 2. W związku z tym w kroku 2, zwracana jest odpowiedni zestaw rekordów. W tym celu należy użyć kodu podobne do poniższych:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Inne obejście polega na utworzeniu program obsługi zdarzeń dla elementu ObjectDataSource s `RowDeleted` zdarzenia i ustawić `AffectedRows` właściwości na wartość 1. Po usunięciu rekordu w kroku 1 (ale przed jego ponowne pobranie danych w kroku 2), aktualizacje widoku GridView jego `PageIndex` właściwości, jeśli co najmniej jeden wiersz został dotknięty wykonać operację. Jednak `AffectedRows` właściwość nie została ustawiona przez element ObjectDataSource i dlatego ten krok zostanie pominięty. Jest jednym ze sposobów zawierają to krok wykonywany ręcznie ustawić `AffectedRows` właściwości, jeśli operacja usuwania zakończy się pomyślnie. Można to zrobić przy użyciu kodu podobne do poniższych:


[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

Kod dla obu tych programów obsługi zdarzeń można znaleźć klasy związane z kodem `EfficientPaging.aspx` przykład.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Porównanie wydajności domyślne i niestandardowe stronicowania

Ponieważ stronicowania niestandardowego pobiera tylko wymagane rekordy, natomiast zwraca domyślną stronicowania *wszystkie* rekordów dla każdej strony wyświetlany, jego s wyczyść czy stronicowania niestandardowego jest bardziej efektywne niż domyślne stronicowania. Jednak samo jak bardziej efektywnego jest stronicowania niestandardowego? Jakiego rodzaju wzrost wydajności, można wyświetlić, przenosząc z domyślnego stronicowania do stronicowania niestandardowego?

Niestety, s nie dostosowane do wszystkich odpowiedzi w tym miejscu. Bardziej wydajne zależy od wielu czynników, najbardziej widocznym dwa liczba rekordów jest stronicowana za pośrednictwem i obciążenia są umieszczane na kanały bazy danych serwera i komunikacji między serwerem sieci web a serwerem bazy danych. Dla małych tabel z kilku dozen rekordów różnicy wydajności może być nieznaczny. Dla dużych tabel z tysiącami setki tysięcy wierszy, jednak wydajność różnica polega na tym ostrych.

Artykuł min, [stronicowania niestandardowego w programie ASP.NET 2.0 z programu SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), zawiera niektóre testy wydajności został uruchomiony wykazują różnic w wydajności między te dwie metody stronicowania podczas stronicowania za pośrednictwem tabeli bazy danych z 50 000 rekordów. W tych testach zbadana I czas do wykonania zapytania na poziomie serwera SQL (przy użyciu [SQL Profiler](https://msdn.microsoft.com/en-us/library/ms173757.aspx)) i na stronie ASP.NET przy użyciu [s śledzenia programu ASP.NET](https://msdn.microsoft.com/en-US/library/y13fw6we.aspx). Należy pamiętać, że te testy były uruchamiane na Moje pole programowanie z jednego aktywnego użytkownika i w związku z tym są unscientific i nie imitują wzorców obciążenia typowe witryny sieci Web. Niezależnie od tego wyniki ilustrują względną różnice w czasie wykonywania w domyślnej i niestandardowej stronicowania podczas pracy z wystarczająco duże ilości danych.


|  | **Średni Czas trwania (s)** | **Odczytuje** |
| --- | --- | --- |
| **Domyślne stronicowania SQL profilera** | 1.411 | 383 |
| **Niestandardowe stronicowania SQL Profiler** | 0.002 | 29 |
| **Domyślne stronicowania ASP.NET śledzenia** | 2.379 | *N/D* |
| **Niestandardowe śledzenia ASP.NET stronicowania** | 0.029 | *N/D* |


Jak widać, pobierania określonej strony danych średnio wymagane 354 mniej odczyty i zostało ukończone w ułamku czasu. Na stronie ASP.NET niestandardowe strony był w stanie mają być renderowane w pobliżu 1-100<sup>th</sup> czasu zajęło, używając domyślnego stronicowania. Zobacz [Moje artykułu](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) Aby uzyskać więcej informacji na temat tych wyników, wraz z kodem i bazy danych można pobrać do odtworzenia tych testów we własnym środowisku.

## <a name="summary"></a>Podsumowanie

Domyślne stronicowania jest proste wdrożenie wyboru tylko pole wyboru Włącz stronicowanie w danych sieci Web kontroli s tagu, ale takie prostota odbywa się kosztem wydajności. Z domyślnego stronicowania, gdy użytkownik zażąda dowolnej strony danych *wszystkie* rekordy są zwracane, nawet jeśli mogą być wyświetlane tylko niewielka część je. Element ObjectDataSource zwalczania to zmniejszenie wydajności, oferuje alternatywne stronicowania niestandardowego opcji stronicowania.

Podczas stronicowania niestandardowego poprawia domyślnego stronicowania problemy z wydajnością s pobierając tylko te rekordy, które muszą zostać wyświetlone, jego s więcej wysiłku do implementowania stronicowania niestandardowego. Po pierwsze zapytania musi być napisana, który prawidłowo (i efektywnie) uzyskuje dostęp do konkretnego podzestawu żądanych rekordów. Można to zrobić na kilka różnych sposobów; co mamy zbadane, w tym samouczku jest użycie programu SQL Server 2005 s nowego `ROW_NUMBER()` funkcji do poziomu powoduje, a następnie aby zwracać tylko te wyniki, których klasyfikacji mieści się w określonym zakresie. Ponadto należy dodać środki do określenia całkowita liczba rekordów jest stronicowana za pośrednictwem. Po utworzeniu tych metod DAL i logiki warstwy Biznesowej, również należy skonfigurować element ObjectDataSource, dzięki czemu może ustalić, ile całkowita liczba rekordów są trwa stronicowanej za pośrednictwem i poprawnie można przekazać wartości Start indeks wiersza i maksymalna liczba wierszy do logiki warstwy Biznesowej.

Podczas implementowania stronicowania niestandardowego wymagają wielu kroków i nie niemal najprostszą domyślne stronicowania, stronicowania niestandardowego jest to konieczne, gdy stronicowania za pośrednictwem wystarczająco duże ilości danych. Jak zbadać wyników stronicowania pokazane, niestandardowe można pozostawia sekund poza czas renderowania stron ASP.NET i rozjaśnić obciążenia na serwerze bazy danych przez jedną lub więcej rzędów.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](paging-and-sorting-report-data-cs.md)
[dalej](sorting-custom-paged-data-cs.md)
