---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Wprowadzenie do składnika ASP.NET Web Pages — wyświetlanie danych | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek pokazuje, jak utworzyć bazę danych w programie WebMatrix i sposób wyświetlania danych bazy danych na stronie, gdy używasz stron sieci Web platformy ASP.NET (Razor). Przyjęto założenie, y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 6c66e5fb0a1a49da411286e19c7954f83055c3fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Wprowadzenie do strony sieci Web ASP.NET - wyświetlanie danych
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek pokazuje, jak utworzyć bazę danych w programie WebMatrix i sposób wyświetlania danych bazy danych na stronie, gdy używasz stron sieci Web platformy ASP.NET (Razor). Przyjęto założenie, że zostały wykonane serii za pomocą [wprowadzenie do programowania stron sieci Web ASP.NET](../introducing-razor-syntax-c.md).
> 
> Zawartość:
> 
> - Jak utworzyć bazę danych i tabel bazy danych za pomocą narzędzi programu WebMatrix.
> - Jak dodać dane do bazy danych za pomocą narzędzi programu WebMatrix.
> - Jak wyświetlać dane z bazy danych na stronie.
> - Sposób uruchamiania poleceń SQL w programie ASP.NET Web Pages.
> - Dostosowywanie `WebGrid` pomocnika, aby zmienić sposób wyświetlania danych i dodać stronicowania i sortowania.
>   
> 
> Funkcje/technologie omówione:
> 
> - Narzędzia bazy danych programu WebMatrix.
> - `WebGrid` Pomocnik.


## <a name="what-youll-build"></a>Jakie będzie kompilacji

Poprzednie samouczka zostały wprowadzone do stron sieci Web platformy ASP.NET (*.cshtml* plików), aby podstawowe informacje o składni Razor i pomocników. W tym samouczku będzie rozpocząć tworzenie aplikacji rzeczywistych sieci web, które będzie używane w pozostałej części serii. Aplikacja jest aplikacji filmu proste, która umożliwia wyświetlanie, dodawanie, zmienianie i usuwania informacji o filmów.

Po zakończeniu korzystania z tego samouczka, będzie można wyświetlić listę film, która wygląda jak ta strona:

![Wyświetl WebGrid, który zawiera parametry ustawioną nazwy klas CSS](displaying-data/_static/image1.png)

Jednak aby rozpocząć, należy utworzyć bazę danych.

## <a name="a-very-brief-introduction-to-databases"></a>Bardzo krótkie wprowadzenie do bazy danych

W tym samouczku będzie udostępniać tylko briefest wprowadzenie do bazy danych. Jeśli masz doświadczenie w bazie danych, możesz pominąć tę sekcję krótki.

Baza danych zawiera co najmniej jednej tabeli, które zawierają informacje &mdash; na przykład tabel dla klientów, zamówienia i dostawców, lub dla uczniów lub studentów, nauczycieli, klasy i klas. Strukturę tabeli bazy danych przypomina arkusz kalkulacyjny. Wyobraź sobie książki adresowej typowych. Dla każdego wpisu w książce adresowej (to znaczy, że dla każdej osoby) ma kilka rodzajów informacji, takich jak imię, nazwisko, adres, adres e-mail i numer telefonu.

![Przykładowa tabela bazy danych jako proste siatki](displaying-data/_static/image2.png)

(Wiersze są czasami określane jako *rekordów*, i kolumny są czasami określane jako *pola*.)

W przypadku większości tabel bazy danych Tabela musi mieć kolumnę zawierającą unikatową wartość, takich jak numer klienta, numer konta i tak dalej. Ta wartość jest znana jako tabeli *klucz podstawowy*, i używa ich do identyfikacji każdego wiersza w tabeli. W tym przykładzie w kolumnie identyfikator jest kluczem podstawowym w poprzednim przykładzie książki adresowej.

Większość zadań wykonywanych w aplikacji sieci web obejmuje odczytu informacji z bazy danych programu i wyświetlanie go na stronie. Zostanie również często zbierania informacji od użytkowników i dodaj go do bazy danych lub będzie modyfikowania rekordów, które już znajdują się w bazie danych. (Firma Microsoft będzie obejmować wszystkie te operacje w ramach tego samouczka zestawu.)

Praca z bazy danych może być enormously złożona i może wymagać specjalistyczną wiedzę. Dla tego samouczka zestawu należy jednak zapoznać tylko podstawowe pojęcia, które wszystkie opisano w trakcie pracy.

## <a name="creating-a-database"></a>Tworzenie bazy danych

Program WebMatrix zawiera narzędzia, które ułatwiają tworzenie bazy danych oraz do tworzenia tabel w bazie danych. (Struktury bazy danych jest określany jako bazy danych *schematu*.) Dla tego zestawu samouczka, utworzysz bazę danych, która ma tylko jedną tabelę w nim &mdash; filmów.

Otwórz program WebMatrix, jeśli jeszcze tego nie zrobiono, a następnie otwórz witrynę WebPagesMovies utworzoną w poprzedniej samouczka.

W okienku po lewej stronie kliknij **bazy danych** obszaru roboczego.

![Karty Obszar roboczy bazy danych programu WebMatrix](displaying-data/_static/image3.png)

Zmiany wstążki Pokaż zadania związane z bazy danych. Na wstążce kliknij **nową bazę danych**.

![Przycisk nową bazę danych na wstążce programu WebMatrix](displaying-data/_static/image4.png)

Program WebMatrix tworzy bazę danych programu SQL Server CE ( *sdf* pliku) który ma taką samą nazwę jak witryny &mdash; *WebPagesMovies.sdf*. (Nie w tym w tym miejscu, ale można zmienić nazwę pliku na niczego Ci się podoba, tak długo, jak długo ma *sdf* rozszerzenia.)

## <a name="creating-a-table"></a>Tworzenie tabeli

Na wstążce kliknij **nową tabelę**. Program WebMatrix otwiera projektanta tabel na nowej karcie. (Jeśli opcja Nowa tabela jest niedostępna, upewnij się, że nowa baza danych jest wybrane w widoku drzewa po lewej stronie.)

![Przycisk "Nowa tabela" na wstążce programu WebMatrix](displaying-data/_static/image5.png)

W polu tekstowym u góry (gdzie znaku wodnego mówi "Wprowadź nazwę tabeli") wprowadź "Filmy".

![Wprowadzanie nazwy tabeli w projektancie bazy danych programu WebMatrix](displaying-data/_static/image6.png)

Okienku poniżej nazwy tabeli jest, gdzie został zdefiniowany poszczególnych kolumn. Dla *filmy* tabeli w tym samouczku utworzysz tylko kilka kolumn: *identyfikator*, *tytuł*, *Genre*, i *roku*.

W **nazwa** wprowadź "ID". Wprowadzenie wartości w tym miejscu aktywuje wszystkie formanty dla nowej kolumny.

Karty, aby **— typ danych** listę i wybierz **int**. Ta wartość określa, czy w kolumnie identyfikator będzie zawierać danych liczb całkowitych (numer).

> [!NOTE]
> Nie będzie nazywamy go jakąkolwiek więcej tutaj (znacznie), ale standardowe gestów klawiatury systemu Windows umożliwia nawigowanie w tej siatce. Na przykład można karcie między polami, może po prostu zacznij wpisywać ciąg w celu wybierz element na liście i tak dalej.


Karta poza **wartość domyślną** pola (to znaczy, pozostaw to pole puste). Karty, aby **jest kluczem podstawowym** pole wyboru i zaznacz je. Ta opcja nakazuje bazy danych *identyfikator* kolumna będzie zawierać dane, które identyfikuje poszczególnych wierszy. (To znaczy każdy wiersz będzie mieć unikatową wartość w kolumnie identyfikator, który umożliwia znalezienie tego wiersza.)

Wybierz **tożsamości jest** opcji. Ta opcja nakazuje bazy danych, czy powinien automatycznie generować kolejny numer sekwencyjny dla każdego nowego wiersza. ( **Tożsamości jest** opcja działa tylko wtedy, gdy wybrano również **jest kluczem podstawowym** opcję.)

Kliknij w następnym wierszu siatki, lub naciśnij klawisz Tab dwukrotnie w celu zakończenia bieżącego wiersza. Albo gestu zapisuje bieżący wiersz i rozpoczyna się kolejny. Zwróć uwagę, że **wartość domyślną** kolumny teraz mówi **Null**. (Wartość null jest wartością domyślną dla wartości domyślnej, więc do speak).

Po zakończeniu definiowania nowej **identyfikator** kolumny, Projektant będzie wyglądać na następującej ilustracji:

![Projektant baz danych programu WebMatrix po zdefiniowaniu w kolumnie Identyfikator tabeli filmy](displaying-data/_static/image7.png)

Aby utworzyć następnej kolumnie, kliknij pole w **nazwa** kolumny. Wprowadź "Title" dla kolumny, a następnie wybierz **nvarchar** dla **— typ danych** wartość. Część "var" **nvarchar** bazy danych informuje, że dane dla tej kolumny zostaną ciąg, którego rozmiar może się różnić od rekordu do rekordu. (Prefiks "n" reprezentuje "national", która wskazuje, czy pole może przechowywać dane znakowe dla dowolnego alfabetu lub zapisywanie systemu — to znaczy pole zawiera dane Unicode.)

Po wybraniu **nvarchar**, zostanie wyświetlone okno innego, za pomocą którego można wprowadzić maksymalną długość pola. Wprowadź 50, przy założeniu, że nie będzie można pracować w tym samouczku tytuł filmu będzie więcej niż 50 znaków.

Pomiń **wartość domyślną** i wyczyść **Zezwalaj na wartości null** opcji. Nie ma bazy danych, aby umożliwić filmy wprowadzane do bazy danych, które nie mają tytuł.

Gdy ukończysz pracę i przejście do następnego wiersza projektanta wygląda na tej ilustracji:

![Projektant baz danych programu WebMatrix po zdefiniowaniu kolumnę tabeli filmy](displaying-data/_static/image8.png)

Powtórz te kroki, aby utworzyć kolumnę o nazwie "Rodzaju", z wyjątkiem długość, ustaw ją na właśnie 30.

Utwórz inną kolumnę o nazwie "Year". Dla typu danych, wybierz **nchar** (nie **nvarchar**) i ustawić długości na 4. Rok możesz zacząć korzystać numer 4-cyfrowego, takich jak "1995" lub "2010", więc nie wymagają kolumny o zmiennej długości.

Oto, jak wygląda gotowego projektu:

![Projektant baz danych programu WebMatrix po wszystkich pól zdefiniowanych dla tabeli filmy](displaying-data/_static/image9.png)

Naciśnij klawisze Ctrl + S lub kliknij przycisk **zapisać** przycisk paska narzędzi Szybki dostęp. Zamknij projektanta bazy danych przez zamknięcie karty.

## <a name="adding-some-example-data"></a>Dodawanie niektóre przykładowe dane

Później w tym samouczku utworzysz strony, których można wprowadzać nowości w formularzu. Jednak obecnie można dodać niektóre przykładowe dane, które można następnie wyświetlić na stronie.

W **bazy danych** obszaru roboczego w programie WebMatrix, zwróć uwagę, że istnieje drzewa, który pokazuje *sdf* wcześniej utworzony plik. Otwórz węzeł dla nowego *sdf* pliku, a następnie otwórz **tabel** węzła.

![Obszar roboczy bazy danych programu WebMatrix z drzewa otwórz tabelę filmy](displaying-data/_static/image10.png)

Kliknij prawym przyciskiem myszy **filmy** węzeł, a następnie wybierz **danych**. Program WebMatrix otwiera siatki, których można wprowadzać dane *filmy* tabeli:

![Siatka zapisu bazy danych w programie WebMatrix (pusta)](displaying-data/_static/image11.png)

Kliknij przycisk **tytuł** kolumny, a następnie wprowadź "Gdy Harry spełnione Zosi". Przenieś do **Genre** kolumny (możesz użyć klawisza Tab), a następnie wprowadź "Sceny miłosne Komedia". Przenieś do **roku** kolumny, a następnie wprowadź "1989":

![Siatka zapisu bazy danych w programie WebMatrix z jeden rekord](displaying-data/_static/image12.png)

Naciśnij klawisz Enter, a program WebMatrix jest zapisywany nowy filmu. Zwróć uwagę, że **identyfikator** kolumny zostały wypełnione.

![Siatka wpis bazy danych w programie WebMatrix jeden rekord i identyfikator wygenerowany automatycznie](displaying-data/_static/image13.png)

Wprowadź inny movie (na przykład, "usunięty z Wind", "Teatralne", "1939"). W kolumnie identyfikator jest wprowadzone ponownie:

![Siatka wpis bazy danych w programie WebMatrix z dwa rekordy i identyfikatorów generowanych automatycznie](displaying-data/_static/image14.png)

Wprowadź trzeci movie (na przykład "Ghostbusters", "Komedia"). Eksperyment, pozostawienie **roku** kolumny puste, a następnie naciśnij klawisz Enter. Ponieważ użytkownik usunął zaznaczenie **Zezwalaj na wartości null** opcji, pokazuje błąd, bazy danych:

![Błąd 'Nieprawidłowe dane' wyświetlane, gdy wartość w kolumnie wymagane jest puste](displaying-data/_static/image15.png)

Kliknij przycisk **OK** przejść wstecz i Usuń wpis (roku "Ghostbusters" jest 1984) i naciśnij klawisz Enter.

Wypełnij kilka filmów, dopóki nie uzyskasz 8 lub tak. (Wprowadzanie 8 ułatwia posługiwanie się stronicowania później. Ale jeśli jest zbyt wiele, wprowadź kilka teraz). Nie ma znaczenia, rzeczywiste dane.

![Siatka zapisu bazy danych w programie WebMatrix z albo rekordów](displaying-data/_static/image16.png)

Jeśli wprowadzono wszystkie filmy bez żadnych błędów, sekwencyjne są wartości Identyfikatora. Próbowano zapisać rekord niekompletne film, numery identyfikatorów może być kolejne. Jeśli tak, że jest poprawny. Liczby nie mają żadnego związanego z używaniem znaczenia, a jedynym elementem, który jest ważne jest, są one unikatowe w *filmy* tabeli.

Zamknij kartę zawierającą projektanta bazy danych.

Teraz można włączyć do wyświetlania tych danych na stronie sieci web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Wyświetlanie danych na stronie przy użyciu Pomocnika WebGrid

Aby wyświetlić dane na stronie, możesz zacząć używać `WebGrid` pomocnika. Tego pomocnika powoduje wyświetlenie w siatce lub tabeli (wierszy i kolumn). Jak można zauważyć, będziesz w stanie Aktualizuj siatkę z formatowanie i inne funkcje.

Aby uruchomić siatki, będzie konieczne Napisz kilka wierszy kodu. Te kilka wierszy będzie służyć jako rodzaj wzorca dla prawie wszystkich dostępu do danych, jak w tym samouczku.

> [!NOTE]
> Faktycznie oferują wiele opcji wyświetlania danych na stronie; `WebGrid` pomocnika jest tylko jedna. Wybraliśmy go w tym samouczku, ponieważ jest najprostszym sposobem wyświetlenia danych i jest rozsądnych elastyczne. W zestawie samouczek dalej zobaczysz sposób użycia więcej "manual" sposób pracy z danymi na tej stronie, co pozwala na bardziej bezpośrednią kontrolę nad sposób wyświetlania danych.


W lewym okienku w programie WebMatrix, kliknij przycisk **pliki** obszaru roboczego.

Nowe bazy danych utworzonej znajduje się w *aplikacji\_danych* folderu. Jeśli folder już nie istnieje, program WebMatrix jej utworzenia nowej bazy danych. (Folder może występować Jeśli wcześniej została zainstalowana pomocników.)

W widoku drzewa wybierz katalog główny witryny sieci Web. Musisz wybrać katalog główny witryny sieci Web; w przeciwnym razie nowy plik może być dodany do aplikacji\_folderem danych.

Na wstążce kliknij **nowy**. W **wybierz typ pliku** wybierz **CSHTML**.

W **nazwa** , podaj nazwę nowej strony "Movies.cshtml":

![Okno dialogowe "Wybierz typ pliku" przedstawiający stronę "Filmy"](displaying-data/_static/image17.png)

Kliknij przycisk **OK** przycisku. Program WebMatrix umożliwia otwarcie nowego pliku z niektóre elementy szkielet. Najpierw będzie napisanie kodu, pobrać dane z bazy danych. Następnie dodasz kod znaczników do strony, aby rzeczywiście wyświetlania danych.

### <a name="writing-the-data-query-code"></a>Pisanie kodu zapytania danych

W górnej części strony między `@{` i `}` znaków, wprowadź poniższy kod. (Upewnij się, wprowadź ten kod pomiędzy otwierającym, a klamrowe nawiasy zamykające).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

Pierwszy wiersz otwiera bazy danych, który został utworzony wcześniej, który jest zawsze pierwszym krokiem przed wykonaniem coś z bazą danych. Można sprawdzić `Database.Open` nazwę metody, można otworzyć bazy danych. Należy zauważyć, że nie zawierają *sdf* w nazwie. `Open` — Metoda przyjęto założenie, że jest szuka *sdf* pliku (oznacza to, *WebPagesMovies.sdf*) oraz że *sdf* plik znajduje się w *aplikacji\_ Dane* folderu. (Zauważyć firma Microsoft, który wcześniej *aplikacji\_danych* folder jest zarezerwowany; ten scenariusz jest jednym z miejsc, w którym ASP.NET sprawia, że założenia o tej nazwie.)

Gdy baza danych jest otwarta, odwołanie do niej są umieszczane w zmiennej o nazwie `db`. (Która może być nazwane niczego.) `db` Zmienna jest sposób zostanie wyświetlone okno interakcji z bazą danych.

Drugi wiersz faktycznie pobiera danych w bazie danych przy użyciu `Query` metody. Zwróć uwagę, jak działa ten kod: `db` zmienna odwołuje się do otwartej bazy danych, i wywołuje `Query` metody przy użyciu `db` zmiennej (`db.Query`).

Sama kwerenda jest SQL `Select` instrukcji. (Trochę tła o SQL, zobacz wyjaśnienie później). W instrukcji `Movies` identyfikuje tabeli w zapytaniu. `*` Znaku określa zapytanie powinno zwrócić wszystkie kolumny z tabeli. (Możesz można również listy kolumn oddzielnie, oddzielonych przecinkami.)

Wyniki zapytania, jeśli taki występuje, są zwracane i udostępniona w `selectedData` zmiennej. Ponownie zmienna może być nazwane żadnych czynności.

Na koniec trzeciego wiersza informuje ASP.NET, czy chcesz użyć wystąpienia `WebGrid` pomocnika. Możesz utworzyć (*wystąpienia*) obiekt pomocnika za pomocą `new` — słowo kluczowe i przekaż go wyników zapytania za pomocą `selectedData` zmiennej. Nowy `WebGrid` obiektu oraz wyniki kwerendy bazy danych są udostępniane w `grid` zmiennej. Wynik będziesz potrzebować na platformie chwilę, aby rzeczywiście wyświetlania danych na stronie.

Na tym etapie bazy danych zostało otwarte, zaakceptowania danych ma i zostały przygotowane `WebGrid` pomocnika za pomocą tych danych. Następnie jest utworzenie znaczników na stronie.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL jest językiem, który jest używany w większości relacyjnych baz danych do zarządzania danymi w bazie danych. Zawiera polecenia, które umożliwiają pobieranie danych i zaktualizować go i umożliwiające tworzenie, modyfikowanie i zarządzanie dane w tabelach bazy danych. SQL różni się od języka programowania (na przykład C#). Poinformuj bazy danych chcesz SQL, i jest zadanie bazy danych, aby dowiedzieć się, jak pobrać dane lub wykonać zadania. Poniżej przedstawiono przykłady niektórych poleceń SQL i co zrobić:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Pierwszy `Select` instrukcji pobiera wszystkie kolumny (określonego przez `*`) z *filmy* tabeli.
> 
> Drugi `Select` instrukcji pobiera kolumny Identyfikatora, nazwy i Price z rekordów w *produktu* tabeli, których ceny wartość kolumny jest więcej niż 10. Polecenie zwraca wyniki w kolejności alfabetycznej na podstawie wartości w kolumnie Nazwa. Jeśli żadne rekordy nie odpowiadają kryteria cen, to polecenie zwraca pusty zestaw.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> To polecenie Wstawia nowy rekord w *produktu* tabeli ustawienie nazwy kolumny "Croissant" Opis kolumnę "A niestabilnym radością" i ceny 1.99.
> 
> Należy zauważyć, że jest określając wartości nieliczbowe wartość jest ujęta w znaki apostrofu (nie znaki cudzysłowu, tak jak C#). Używamy tych cudzysłowów wokół wartości tekstowe lub daty, ale nie wokół cyfry.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> To polecenie usuwa rekordy w *produktu* tabeli, którego kolumna Data wygaśnięcia jest wcześniejsza niż 1 stycznia 2008. (Polecenia założono, że *produktu* Tabela oczywiście ma takie kolumny.) Tutaj podano datę w formacie MM/DD/RRRR, ale powinny być wprowadzane w formacie, który jest używany dla ustawień regionalnych użytkownika.
> 
> `Insert` i `Delete` poleceń nie zwracać zestawów wyników. Zamiast tego zwracają numer, informujący o tym, jak wiele rekordów wpłynęła na polecenie.
> 
> Dla niektórych z tych operacji (takich jak wstawianie i usuwanie rekordów) proces, który żąda operacji musi mieć odpowiednie uprawnienia w bazie danych. To dlatego dla baz danych produkcyjnych często konieczne podanie nazwy użytkownika i hasła podczas łączenia z bazą danych.
> 
> Istnieje wielu poleceń SQL, ale wszystkie one wykonać wzorzec, takich jak polecenia widocznej w tym miejscu. Poleceń SQL służy do tworzenia tabel bazy danych, liczbę rekordów w tabeli obliczyć ceny i wykonywać wiele kolejnych operacji.


### <a name="adding-markup-to-display-the-data"></a>Dodawanie znaczników do wyświetlania danych

Wewnątrz `<head>` elementu, zawartość zestawu `<title>` elementu "Filmy":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Wewnątrz `<body>` element strony, Dodaj następujący kod:

[!code-html[Main](displaying-data/samples/sample3.html)]

To wszystko. `grid` Wartość utworzone podczas tworzenia zmiennej to `WebGrid` obiektu w kodzie wcześniej.

W widoku drzewa programu WebMatrix, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Uruchom w przeglądarce**. Zostanie wyświetlony ekran podobny do tej strony:

![Domyślnie dane wyjściowe pomocnika WebGrid tabeli filmy](displaying-data/_static/image18.png)

Kliknij łącze nagłówek kolumny, aby sortować według tej kolumny. Możliwość sortować, klikając odpowiedni nagłówek jest funkcją, która jest wbudowana w **WebGrid** pomocnika.

`GetHtml` Metoda, jak jego nazwa sugeruje, generuje kod znaczników, który wyświetla dane. Domyślnie `GetHtml` metoda generuje HTML `<table>` elementu. (Jeśli chcesz, możesz sprawdzić renderowanie przez patrzeć źródło strony w przeglądarce.)

## <a name="modifying-the-look-of-the-grid"></a>Modyfikowanie wyglądu siatki

Przy użyciu `WebGrid` pomocnika tak tak samo jak jest łatwe, ale jest zwykły wynikami wyszukiwania. `WebGrid` Pomocnika ma szerokiej gamy opcje umożliwiające sterowanie sposób wyświetlania danych. Istnieje zbyt dużo, aby zapoznać się z tego samouczka, ale w tej sekcji zostanie dają pogląd, niektóre z tych opcji. Kilka dodatkowych opcji zostaną opisane w kolejnych samouczkach z tej serii.

### <a name="specifying-individual-columns-to-display"></a>Określenie poszczególnych kolumn do wyświetlenia

Aby rozpocząć, można określić, czy mają być wyświetlane tylko niektóre kolumny. Domyślnie, jak zostały przeczytane na siatce są wyświetlane wszystkie cztery kolumny *filmy* tabeli.

W *Movies.cshtml* pliku, Zastąp `@grid.GetHtml()` kod znaczników, który właśnie dodano następującym kodem:

[!code-css[Main](displaying-data/samples/sample4.css)]

Mówić pomocnika kolumn do wyświetlenia, obejmują `columns` parametr `GetHtml` — metoda i przekazać w kolekcji kolumn. W kolekcji należy określić każdej kolumny w celu uwzględnienia. Określ poszczególne kolumny do wyświetlenia w tym `grid.Column` obiektów i podaj nazwę kolumny danych ma. (Te kolumny muszą być zawarte w wynikach zapytania SQL — `WebGrid` pomocnika nie może wyświetlić kolumny, które nie zostały zwrócone przez zapytanie.)

Uruchom *Movies.cshtml* strony w przeglądarce ponownie i tym razem zostanie wyświetlony ekran podobny do następującego (Zauważ, że w kolumnie Identyfikator nie jest wyświetlana):

![Wyświetl WebGrid przedstawiający tylko wybranych kolumn](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Zmiana wyglądu siatki

Istnieje kilka więcej opcji wyświetlania kolumn, niektóre z nich zostaną przedstawione w kolejnych samouczkach w tym zestawie. Obecnie w tej sekcji przedstawiono sposoby, w którym można stylów siatki jako całość.

Wewnątrz `<head>` części strony, bezpośrednio przed zamknięciem `</head>` tagu, należy dodać następujące `<style>` elementu:

[!code-css[Main](displaying-data/samples/sample5.css)]

Ten kod znaczników CSS definiuje klasy o nazwie `grid`, `head`i tak dalej. Można również wprowadzić te definicje stylu w oddzielnej *.css* plików i połączona ze stroną. (W rzeczywistości należy to zrobić w dalszej części tego samouczka zestawu.) Jednak aby ułatwić czynności w tym samouczku, są one w tej samej stronie, która wyświetla dane.

Teraz możesz uzyskać `WebGrid` pomocnika do używania tych klas stylu. Pomocnik ma wiele właściwości (na przykład `tableStyle`) właśnie w tym celu — przypisać nazwę klasy stylów CSS do nich, i że nazwa klasy jest renderowane jako część znaczników, który jest renderowany przez pomocnika.

Zmień `grid.GetHtml` znaczników, aby teraz wygląda ten kod:

[!code-css[Main](displaying-data/samples/sample6.css)]

Nowości w tym miejscu jest, że zostały dodane `tableStyle`, `headerStyle`, i `alternatingRowStyle` parametry `GetHtml` metody. Te parametry zostały ustawione na nazwy stylów CSS, które dodano kilka minut temu.

Uruchom strony, a teraz Zobacz siatki, która wygląda znacznie mniej zwykły niż przed:

![Wyświetl WebGrid, który zawiera parametry ustawioną nazwy klas CSS](displaying-data/_static/image20.png)

Aby sprawdzić, jakie `GetHtml` wygenerowane metody, można sprawdzić źródło strony w przeglądarce. Nie możemy przejść do szczegółów w tym miejscu, ale istotne jest, określając parametry, takie jak `tableStyle`, spowodowane siatki mają być Generowanie tagi HTML podobne do poniższych:

`<table class="grid">`

`<table>` Miał znacznik `class` dodany atrybut, który odwołuje się do jednego z reguły CSS, które zostały wcześniej dodane. Ten kod przedstawia podstawowy wzorzec &mdash; różnych parametrów `GetHtml` metody umożliwiają możesz odwołanie CSS klas, że z nich metoda generuje następnie wraz ze znaczników. Co należy zrobić z klas CSS zależy od użytkownika.

## <a name="adding-paging"></a>Dodawanie stronicowania

Jako ostatnie zadanie dla tego samouczka warto stronicowania siatki. W tej chwili jest żadnych problemów, aby wyświetlić wszystkie filmów jednocześnie. Jednak jeśli dodano setki filmów wyświetlania stron, jak długo.

W kodzie strony, zmień wiersz, który tworzy `WebGrid` obiektu poniższy kod:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Jedyną różnicą z przed jest, że zostały dodane `rowsPerPage` parametr, który ma ustawioną wartość 3.

Uruchom strony. Siatka wyświetla 3 wiersze na czas, oraz łączy nawigacji, które umożliwiają przeglądanie filmów w bazie danych:

![Wyświetl WebGrid z stronicowania](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Powtarzający się dalej

W następnym samouczku nauczysz się, jak pobrać dane wejściowe użytkownika w postaci za pomocą kodu Razor i C#. Pole wyszukiwania będzie dodany do strony filmów, dzięki czemu można znaleźć filmy tytuł lub rodzaju.

## <a name="complete-listing-for-movies-page"></a>Pełna lista filmów strony

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Poprzednie](intro-to-web-pages-programming.md)
> [dalej](form-basics.md)
