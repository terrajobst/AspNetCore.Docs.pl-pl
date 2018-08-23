---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Wprowadzenie do składnika ASP.NET Web Pages — wyświetlanie danych | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku dowiesz się, jak utworzyć bazę danych w programie WebMatrix oraz jak wyświetlać dane z bazy danych na stronie, gdy używasz stron ASP.NET Web Pages (Razor). Przyjęto założenie, y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 864b9f7826763e307368706116458678abf50d3b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757291"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Wprowadzenie do wzorca ASP.NET Web Pages — wyświetlanie danych
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku dowiesz się, jak utworzyć bazę danych w programie WebMatrix oraz jak wyświetlać dane z bazy danych na stronie, gdy używasz stron ASP.NET Web Pages (Razor). Przyjęto założenie, że zostały wykonane serii za pośrednictwem [wprowadzenie do programowania programu ASP.NET Web Pages](../introducing-razor-syntax-c.md).
> 
> Zawartość:
> 
> - Jak używać narzędzi programu WebMatrix do tworzenia bazy danych i tabel bazy danych.
> - Jak używać narzędzi programu WebMatrix, aby dodać dane do bazy danych.
> - Jak wyświetlić dane z bazy danych na stronie.
> - Sposób uruchamiania poleceń SQL w składniku ASP.NET Web Pages.
> - Jak dostosować `WebGrid` pomocnika, aby zmienić sposób wyświetlania danych i dodać stronicowanie i sortowanie.
>   
> 
> Funkcje/technologie omówione:
> 
> - Narzędzia graficzne bazy danych programu WebMatrix.
> - `WebGrid` Pomocnik.


## <a name="what-youll-build"></a>Jakie będziesz tworzyć

W poprzednim samouczku zostały wprowadzone do stron ASP.NET Web Pages (*.cshtml* plików), podstawowe informacje o składni Razor i Pomocnicy. W tym samouczku będziesz Rozpocznij tworzenie aplikacji rzeczywisty sieci web, którego będziesz używać w pozostałej części tej serii. Aplikacja jest aplikacji proste filmów, która umożliwia wyświetlanie, dodawanie, zmienianie i usuwanie informacje na temat filmów.

Po zakończeniu korzystania z tego samouczka, będzie można wyświetlić listę filmu, która wygląda podobnie do tej strony:

![Wyświetlanie WebGrid, która obejmuje parametry równa nazwy klas CSS](displaying-data/_static/image1.png)

Jednak aby rozpocząć, należy utworzyć bazę danych.

## <a name="a-very-brief-introduction-to-databases"></a>Bardzo krótkie wprowadzenie do bazy danych

W tym samouczku będzie udostępniać tylko briefest wprowadzenie do bazy danych. Jeśli masz doświadczenie w bazie danych, możesz pominąć tę sekcję krótki.

Baza danych zawiera co najmniej jednej tabeli, które zawierają informacje &mdash; na przykład tabele dla klientów, zamówień i dostawców lub dla studentów, nauczycieli, klasy i klas. Strukturalnie tabeli bazy danych jest jak arkusz kalkulacyjny. Wyobraź sobie książki adresowej typowy. Dla każdego wpisu w książce adresowej (oznacza to, że dla każdej osoby nawiązującej) ma kilka rodzajów informacji, takich jak imię, nazwisko, adres, adres e-mail i numer telefonu.

![Przykładowa tabela bazy danych jako prosta siatka](displaying-data/_static/image2.png)

(Wiersze są czasami określane jako *rekordów*, i kolumny są czasami określane jako *pola*.)

W przypadku większości tabel bazy danych Tabela musi mieć kolumnę zawierającą unikatową wartość, na przykład numer klienta, numer konta i tak dalej. Ta wartość jest znana jako tabela *klucz podstawowy*, i używa ich do identyfikacji każdego wiersza w tabeli. W tym przykładzie kolumna Identyfikator jest kluczem podstawowym książki adresowej w poprzednim przykładzie.

Większość zadań, co zrobić w aplikacji sieci web składa się z odczytywania informacji z bazy danych i wyświetlanie go na stronie. Zostanie również często zbierać dane od użytkowników i dodać go do bazy danych lub zmodyfikujesz rekordy, które znajdują się już w bazie danych. (Omówimy wszystkie te operacje, w tym zestawie samouczków.)

Praca bazy danych może być teraz znacznie skomplikowane i wymagać specjalistyczną wiedzę. Dla tego zestawu samouczków masz jednak zapoznać się tylko podstawowymi pojęciami, które zostaną wszystkie wyjaśnione zgodnie z rzeczywistym.

## <a name="creating-a-database"></a>Tworzenie bazy danych

Program WebMatrix zawiera narzędzia, które ułatwiają tworzenie bazy danych i tworzenie tabel w bazie danych. (Struktury bazy danych jest określany jako bazy danych *schematu*.) Dla tego zestawu samouczków, utworzysz bazę danych, która zawiera tylko jedną tabelę &mdash; filmów.

Otwórz program WebMatrix, jeśli jeszcze tego nie zrobiłeś, a następnie otwórz witrynę WebPagesMovies, który został utworzony w poprzednim samouczku.

W okienku po lewej stronie kliknij **bazy danych** obszaru roboczego.

![Karty Obszar roboczy bazy danych programu WebMatrix](displaying-data/_static/image3.png)

Zmiany wstążki, aby wyświetlić zadania związane z bazy danych. Na wstążce kliknij **nową bazę danych**.

![Nowa baza danych przycisk na wstążce programu WebMatrix](displaying-data/_static/image4.png)

Program WebMatrix tworzy bazę danych programu SQL Server CE ( *.sdf* pliku), ma taką samą nazwę jako lokacji &mdash; *WebPagesMovies.sdf*. (Nie zrobi to w tym miejscu, ale można zmienić nazwę pliku do żadnego elementu lubisz, tak długo, jak przedstawiono w nim *.sdf* rozszerzenia.)

## <a name="creating-a-table"></a>Tworzenie tabeli

Na wstążce kliknij **nową tabelę**. Program WebMatrix zostanie otwarty projektant tabel w nowej karcie. (Jeśli opcja Nowa tabela jest niedostępna, upewnij się, że nowa baza danych jest wybrane w widoku drzewa po lewej stronie.)

!["Nowa tabela" przycisk na wstążce programu WebMatrix](displaying-data/_static/image5.png)

W polu tekstowym u góry (gdzie znak wodny mówi "Wprowadź nazwę tabeli") wprowadź "Filmy".

![Wprowadź nazwę tabeli w projektancie bazy danych programu WebMatrix](displaying-data/_static/image6.png)

Określa poszczególnych kolumn, które się w okienku poniżej nazwy tabeli. Dla *filmy* tabeli w tym samouczku utworzysz tylko kilka kolumn: *identyfikator*, *tytuł*, *gatunku*, i *roku*.

W **nazwa** wprowadź "ID". Wprowadź wartość w tym miejscu aktywuje wszystkich kontrolek dla nowej kolumny.

Kartę **— typ danych** i wybierz polecenie **int**. Ta wartość określa, że kolumna Identyfikatora będzie zawierać danych liczb całkowitych (liczba).

> [!NOTE]
> Nie będzie nazywamy je jakąkolwiek więcej informacji znajdziesz tutaj (znacznie), ale standardowa gestów klawiatury Windows można użyć do nawigacji w tej siatce. Na przykład ustawić tabulator między polami, można po prostu zacznij pisać, aby wybrać element na liście i tak dalej.


Karta ostatnie **wartości domyślnej** pole (oznacza to, pozostaw to pole puste). Kartę **jest kluczem podstawowym** pole wyboru, a następnie wybierz ją. Ta opcja nakazuje bazy danych *identyfikator* kolumna będzie zawierać dane, który identyfikuje poszczególne wiersze. (Czyli każdy wiersz będą mieć unikatową wartość w kolumnie identyfikator, która umożliwia znalezienie tego wiersza.)

Wybierz **tożsamości jest** opcji. Ta opcja nakazuje bazy danych, czy należy automatycznie wygenerować kolejny numer kolejny dla każdego nowego wiersza. ( **Tożsamości jest** opcji działa tylko wtedy, gdy wybrano również **jest kluczem podstawowym** opcję.)

Kliknij następny wiersz siatki, lub naciśnij klawisz Tab dwa razy, aby zakończyć bieżący wiersz. Albo gestu zapisuje bieżący wiersz i rozpoczyna się kolejny. Należy zauważyć, że **wartości domyślnej** kolumny jest teraz wyświetlany komunikat **Null**. (Wartość null jest wartością domyślną dla wartości domyślnej, więc do speak).

Po zakończeniu, definiując nowe **identyfikator** kolumny, Projektant będzie wyglądać na tej ilustracji:

![Projektant baz danych programu WebMatrix po zdefiniowaniu kolumny Identyfikatora dla tabeli filmy](displaying-data/_static/image7.png)

Aby utworzyć w następnej kolumnie, kliknij pole w **nazwa** kolumny. Wprowadź "Title" dla kolumny, a następnie wybierz **nvarchar** dla **— typ danych** wartość. Część "var" **nvarchar** bazy danych informuje, że dane dla tej kolumny będzie ciąg, którego rozmiar może się różnić między rekordami. (Prefiks "n" reprezentuje "national", co oznacza, że pole może zawierać dane znaków dla dowolnego alfabetu lub systemie pisma — oznacza to pole zawiera dane Unicode.)

Po wybraniu **nvarchar**, pojawi się okno innego, za pomocą której można wprowadzić maksymalną długość pola. Wprowadź 50, przy założeniu, że bez tytułu film, w tym samouczku będziesz pracować będzie więcej niż 50 znaków.

Pomiń **wartości domyślnej** i wyczyść **Zezwalaj na wartości null** opcji. Nie ma bazy danych do Zezwalaj na filmy wprowadzane do bazy danych, które nie mają tytuł.

Gdy ukończysz pracę i przejdź do następnego wiersza projektanta wygląda na tej ilustracji:

![Projektant baz danych programu WebMatrix po zdefiniowaniu kolumny Title dla tabeli filmy](displaying-data/_static/image8.png)

Powtórz te kroki, aby utworzyć kolumnę o nazwie "Rodzaju", z wyjątkiem długości, ustaw ją po prostu 30.

Utwórz inną kolumnę o nazwie "Roku." Typ danych wybierz **nchar** (nie **nvarchar**) i ustawić długość na 4. W roku możesz zacząć korzystać 4-cyfrowy numer, takich jak "1995" lub "2010", dzięki czemu nie wymagają kolumny o zmiennym rozmiarze.

Oto jak wygląda gotowego projektu:

![Projektant baz danych programu WebMatrix po wszystkich pól zdefiniowanych dla tabeli filmy](displaying-data/_static/image9.png)

Naciśnij klawisze Ctrl + S, lub kliknij przycisk **Zapisz** przycisku paska narzędzi Szybki dostęp. Zamknięcie karty, aby zamknąć projektanta bazy danych.

## <a name="adding-some-example-data"></a>Dodawanie niektóre przykładowe dane

W dalszej części tej serii samouczków utworzysz strony, w którym możesz wprowadzić nowe filmy w formularzu. Teraz jednak możesz dodać niektóre przykładowe dane, które następnie można wyświetlić na stronie.

W **bazy danych** obszaru roboczego w programie WebMatrix, zwróć uwagę, że istnieje drzewa, która pokazuje *.sdf* pliku została utworzona wcześniej. Otwórz węzeł nowej *.sdf* pliku, a następnie otwórz **tabel** węzła.

![Obszar roboczy bazy danych programu WebMatrix, przy użyciu drzewa, otwarty do tabeli filmy](displaying-data/_static/image10.png)

Kliknij prawym przyciskiem myszy **filmy** węzeł, a następnie wybierz **danych**. Program WebMatrix otwiera siatki, w którym możesz wprowadzić dane dla *filmy* tabeli:

![Siatka zapisu bazy danych w programie WebMatrix (pusty)](displaying-data/_static/image11.png)

Kliknij przycisk **tytuł** kolumny i wpisz "Podczas Harry spełnione Zosi". Przenieś do **gatunku** kolumny (możesz użyć klawisza Tab) i wprowadź "Sceny miłosne Komedia". Przenieś do **roku** kolumny i wpisz "1989":

![Siatka zapisu bazy danych w programie WebMatrix z jednego rekordu](displaying-data/_static/image12.png)

Naciśnij klawisz Enter i programu WebMatrix zapisuje ten nowy film. Należy zauważyć, że **identyfikator** kolumna została wypełniona.

![Siatka wpis bazy danych w programie WebMatrix z jednego rekordu i automatycznie wygenerowany identyfikator](displaying-data/_static/image13.png)

Wprowadź inny film (na przykład "stała się za pomocą wiatru", "Dramat", "1939"). W kolumnie identyfikator jest wprowadzone ponownie:

![Siatka zapisu bazy danych w programie WebMatrix z dwa rekordy i identyfikatorów generowanych automatycznie](displaying-data/_static/image14.png)

Wprowadź trzeci filmu (na przykład "Ghostbusters", "Dokument"). Jako eksperyment, pozostaw **roku** kolumny blank, a następnie naciśnij klawisz Enter. Ponieważ użytkownik niezaznaczoną **Zezwalaj na wartości null** opcji i baza danych zawiera błąd:

![Błąd 'Nieprawidłowe dane' wyświetlane, gdy wartości wymaganej kolumny jest puste](displaying-data/_static/image15.png)

Kliknij przycisk **OK** Przejdź wstecz i naprawić zapisu (jest 1984 roku "Ghostbusters".) i naciśnij klawisz Enter.

Wypełnij kilka filmów, dopóki nie uzyskasz 8, lub tak. (Wprowadzanie 8 sprawia, że łatwiej pracować z stronicowania później. Ale jeśli jest zbyt wiele, należy wprowadzić kilka teraz). Rzeczywiste dane, nie ma znaczenia.

![Siatka zapisu bazy danych w programie WebMatrix z albo rekordów](displaying-data/_static/image16.png)

Po wprowadzeniu wszystkich filmów bez żadnych błędów, identyfikator wartości są sekwencyjne. Podjęto próbę zapisania rekordu niekompletne filmu, numery identyfikatorów może być sekwencyjne. Jeśli tak, to zignorować. Liczby nie mają żadnego znaczenia nieprzerwaną pracę, a jedyną czynnością, które są ważne jest, że są one unikatowe w *filmy* tabeli.

Zamknij kartę zawierającą projektanta bazy danych.

Teraz możesz wyłączyć wyświetlanie tych danych na stronie sieci web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Wyświetlanie danych na stronie sieci przy użyciu Pomocnika WebGrid

Aby wyświetlić dane na stronie sieci, możesz zacząć korzystać `WebGrid` pomocnika. Tego pomocnika generuje wyświetlaną w siatce lub na tabeli (wiersze i kolumny). Jak można zauważyć, będzie możliwe dopracować siatka z formatowania i inne funkcje.

Aby uruchomić siatki, musisz napisać kilka wierszy kodu. Te wiersze kilka będzie służyć jako rodzaju wzorzec prawie cały dostęp do danych, jak w tym samouczku.

> [!NOTE]
> Masz naprawdę wiele opcji do wyświetlania danych na stronie; `WebGrid` pomocy jest tylko jeden. Wybraliśmy ją na potrzeby tego samouczka, ponieważ jest najprostszym sposobem wyświetlania danych i jest jest elastyczny. W zestawie następnego samouczka pojawi się, jak używać więcej "manual" nowy sposób pracy z danymi na stronie, co daje więcej bezpośrednią kontrolę nad sposób wyświetlania danych.


W okienku po lewej stronie w programie WebMatrix, kliknij przycisk **pliki** obszaru roboczego.

Nowe utworzona baza danych znajduje się w *aplikacji\_danych* folderu. Jeśli folder już nie istnieje, program WebMatrix utworzony dla nowej bazy danych. (Folder może występować Jeśli wcześniej została zainstalowana pomocników.)

W widoku drzewa wybierz katalog główny witryny sieci Web. Musisz wybrać katalog główny witryny sieci Web; w przeciwnym razie nowy plik może być dodana do aplikacji\_folderu danych.

Na wstążce kliknij **New**. W **wybierz typ pliku** wybierz **CSHTML**.

W **nazwa** pola, określ nazwę nowej strony "Movies.cshtml":

![Okno dialogowe "Wybierz typ pliku" przedstawiający stronę "Filmy"](displaying-data/_static/image17.png)

Kliknij przycisk **OK** przycisku. Program WebMatrix zostanie otwarty nowy plik niektóre szkielet zawartych. Najpierw przedstawiono tworzenie kodu, aby pobrać dane z bazy danych. Następnie dodasz kod znaczników do strony, aby faktycznie wyświetlić dane.

### <a name="writing-the-data-query-code"></a>Pisanie kodu zapytania danych

W górnej części strony między `@{` i `}` znaków, wprowadź następujący kod. (Upewnij się, wprowadź ten kod między otwierające i zamykające nawiasy klamrowe).

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

Pierwszy wiersz spowoduje otwarcie bazy danych, który został utworzony wcześniej, który jest zawsze pierwszym krokiem przed wykonaniem coś z bazą danych. Poinformuj `Database.Open` nazwę metody bazy danych, aby otworzyć. Należy zauważyć, że nie uwzględniają *.sdf* w nazwie. `Open` Metoda zakłada, że szuka *.sdf* pliku (oznacza to, że *WebPagesMovies.sdf*) oraz że *.sdf* plik znajduje się w *aplikacji\_ Dane* folderu. (Wcześniej mogliśmy zauważyć że *aplikacji\_danych* folder jest zarezerwowany; ten scenariusz jest jednym z miejsc, w którym ASP.NET sprawia, że założeń o tej nazwie.)

Gdy baza danych jest otwarta, odwołanie do niej są umieszczane w zmiennej o nazwie `db`. (Które mogą być nazwane niczego.) `db` Zmienna jest jak użytkownik będzie pozostać interakcji z bazą danych.

Drugi wiersz faktycznie pobiera dane bazy danych przy użyciu `Query` metody. Zwróć uwagę, jak działa ten kod: `db` zmienna odwołuje się do otwartej bazy danych, a następnie wywołuje `Query` metody, używając `db` zmiennej (`db.Query`).

Samo zapytanie jest SQL `Select` instrukcji. (Aby trochę informacji uzupełniających na temat programu SQL, zobacz objaśnienia w dalszej części). W instrukcji `Movies` Określa tabelę, aby wykonać zapytanie. `*` Znak Określa, że zapytanie powinno zwrócić wszystkie kolumny z tabeli. (Można podać również listę kolumn pojedynczo, oddzielonych przecinkami.)

Wyniki zapytania, ewentualnie są zwracane i udostępniona w `selectedData` zmiennej. Ponownie zmiennej może być nazwane niczego.

Na koniec trzeci wiersz informuje ASP.NET, czy chcesz użyć instancji `WebGrid` pomocnika. Możesz utworzyć (*wystąpienia*) obiekt pomocnika za pomocą `new` — słowo kluczowe i przekaż go wyników zapytania za pomocą `selectedData` zmiennej. Nowy `WebGrid` obiektu wraz z wynikami zapytania bazy danych są udostępniane w `grid` zmiennej. Należy wynik za chwilę, aby faktycznie wyświetlić dane na stronie.

Na tym etapie bazy danych został otwarty, trafiła do Ciebie dane chcesz i przygotowaniu `WebGrid` pomocnika z tymi danymi. Następnie jest utworzenie znaczników na stronie.

> [!TIP] 
> 
> **Język Structured Query Language (SQL)**
> 
> SQL jest językiem, który jest używany w większości relacyjnych baz danych do zarządzania danymi w bazie danych. Zawiera polecenia umożliwiające pobieranie danych i zaktualizować go i umożliwiające tworzenie, modyfikowanie i zarządzanie danymi w tabelach bazy danych. SQL różni się od języka programowania (takich jak C#). Przy użyciu języka SQL Poinformuj bazy danych należy, i jest zadanie bazy danych, aby dowiedzieć się, jak można pobrać dane lub wykonać zadanie. Poniżej przedstawiono przykłady niektórych poleceń SQL oraz ich działania:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Pierwszy `Select` instrukcji pobiera wszystkie kolumny (określony przez `*`) z *filmy* tabeli.
> 
> Drugi `Select` instrukcji pobiera kolumny Identyfikatora, nazwy i ceny z rekordów *produktu* tabeli, w której wartość kolumny Cena jest więcej niż 10. Polecenie zwraca wyniki w kolejności alfabetycznej na podstawie wartości w kolumnie Nazwa. Jeśli żadne rekordy nie odpowiadają kryteria ceny, polecenie zwraca pusty zestaw.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> To polecenie Wstawia nowy rekord do *produktu* tabeli, ustawienie nazwy kolumny "Croissant" Opis kolumny do "A niestabilne wzbudzanie" oraz ceny 1,99.
> 
> Należy zauważyć, że określając one wartości nieliczbowe wartość jest ujęta w znaki pojedynczego cudzysłowu (nie znaki cudzysłowu, tak jak w języku C#). Użyjesz tych znaków cudzysłowu wokół wartości tekstowe lub daty, ale nie w całym liczb.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> To polecenie usuwa rekordy w *produktu* tabeli, którego kolumna Data wygaśnięcia jest wcześniejsza niż 1 stycznia 2008. (Polecenia założono, że *produktu* tabela ma kolumnę, oczywiście.) W tym miejscu podano daty w formacie MM/DD/RRRR, ale powinny być wprowadzane w formacie, który jest używany dla ustawień regionalnych.
> 
> `Insert` i `Delete` polecenia nie zwracają zestaw wyników. Zamiast tego zwracają liczbę, która informuje, jak wiele rekordów wpłynęła na polecenie.
> 
> W przypadku niektórych z tych operacji (takich jak wstawianie i usuwanie rekordów) proces, który żąda operacji musi mieć odpowiednie uprawnienia w bazie danych. To dlatego produkcyjnych baz danych, często musisz podać nazwę użytkownika i hasło, po nawiązaniu połączenia z bazą danych.
> 
> Istnieją dziesiątek poleceń SQL, ale wszystkie one wykonać wzorca, takich jak polecenia widocznej w tym miejscu. Można użyć poleceń SQL do tworzenia tabel bazy danych, liczby rekordów w tabeli, Oblicz ceny i wykonuje wiele więcej operacji.


### <a name="adding-markup-to-display-the-data"></a>Dodawanie znaczników do wyświetlania danych

Wewnątrz `<head>` elementu, zawartość zestawu `<title>` elementu "Filmy":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Wewnątrz `<body>` elementu strony, Dodaj następujący kod:

[!code-html[Main](displaying-data/samples/sample3.html)]

To wszystko. `grid` Zmienna jest wartość utworzona podczas tworzenia `WebGrid` obiektu w kodzie wcześniej.

W widoku drzewa w programie WebMatrix, kliknij prawym przyciskiem myszy strony, a następnie wybierz **Uruchom w przeglądarce**. Zostanie wyświetlony podobny do tej strony:

![Domyślne dane wyjściowe pomocnika WebGrid z tabeli filmy](displaying-data/_static/image18.png)

Kliknij link do nagłówka kolumny, aby sortować według tej kolumny. Możliwość sortować, klikając nagłówek jest funkcją, która jest wbudowana w **WebGrid** pomocnika.

`GetHtml` Metody, jak sugeruje nazwa, generuje kod znaczników, który wyświetla dane. Domyślnie `GetHtml` metoda generuje HTML `<table>` elementu. (Jeśli chcesz, możesz sprawdzić renderowania, analizując źródło strony w przeglądarce.)

## <a name="modifying-the-look-of-the-grid"></a>Modyfikowanie wyglądu siatki

Za pomocą `WebGrid` pomocnika, takie jak właśnie zrobił jest łatwe, ale wynikowy ekranu to zwykły. `WebGrid` Pomocnika udostępnia szeroką gamę opcji umożliwiające możesz kontrolować sposób wyświetlania danych. Istnieją zbyt dużo, aby zapoznać się w tym samouczku, ale w tej sekcji zostanie dają pogląd, niektórych z tych opcji. Kilka dodatkowych opcji zostały omówione w kolejnych samouczkach w tej serii.

### <a name="specifying-individual-columns-to-display"></a>Określenie poszczególnych kolumn do wyświetlenia

Aby rozpocząć, można określić, czy chcesz wyświetlić tylko określone kolumny. Domyślnie, zgodnie z wyświetlonymi wcześniej na siatce są wyświetlane wszystkie cztery kolumny z *filmy* tabeli.

W *Movies.cshtml* pliku, Zastąp `@grid.GetHtml()` kod znaczników, który właśnie został dodany następującym kodem:

[!code-css[Main](displaying-data/samples/sample4.css)]

Sprawdzić pomocnika, które kolumny mają być wyświetlane, obejmują `columns` parametr `GetHtml` metody i przekazać w zbiorze kolumn. W tej kolekcji należy określić każdej kolumny do uwzględnienia. Należy określić pojedynczej kolumny do wyświetlenia, umieszczając `grid.Column` obiektu, a następnie przekazać nazwę kolumny danych, które chcesz. (Te kolumny muszą być zawarte w wynikach kwerendy SQL — `WebGrid` pomocnika nie może wyświetlić kolumn, które nie zostały zwrócone przez zapytanie.)

Uruchom *Movies.cshtml* strony w przeglądarce, ponownie i tym razem pojawi się ekran podobny do następującego (należy zauważyć, że żadna kolumna ID jest wyświetlany):

![Wyświetlanie WebGrid przedstawiający tylko zaznaczonych kolumn](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Zmienianie wyglądu siatki

Istnieje kilka więcej opcji wyświetlania kolumn, niektóre z nich zostaną przedstawione w kolejnych samouczkach w tym zestawie. Obecnie w tej sekcji przedstawiono sposoby stylizować siatki jako całości.

Wewnątrz `<head>` części strony, bezpośrednio przed zamykającym `</head>` tag, Dodaj następujący kod `<style>` elementu:

[!code-css[Main](displaying-data/samples/sample5.css)]

Ten kod znaczników CSS definiuje klasy o nazwie `grid`, `head`i tak dalej. Możesz również umieścić te definicje styl w osobnym *.css* pliku i łączem do strony. (W rzeczywistości, należy to zrobić później w tym zestawie samouczków.) Jednak aby umożliwić ich łatwe w tym samouczku, są one w tej samej stronie, który wyświetla dane.

Teraz możesz dostawać `WebGrid` pomocnika do użycia w ramach tych zajęć stylu. Pomocnik ma wiele właściwości (na przykład `tableStyle`) tylko w tym celu — przypisać nazwy klasy stylów CSS do nich i że nazwa klasy jest renderowana jako część znaczników, który jest renderowany przez pomocnika.

Zmiana `grid.GetHtml` znaczników, tak że wygląda podobnie do tego kodu:

[!code-css[Main](displaying-data/samples/sample6.css)]

Co nowego w tym miejscu jest, że dodano `tableStyle`, `headerStyle`, i `alternatingRowStyle` parametry `GetHtml` metody. Te parametry zostały ustawione do nazw stylów CSS, które dodałeś przed chwilą.

Uruchom stronę i tym razem zostanie wyświetlony siatki, która wygląda znacznie mniej zwykły niż przed:

![Wyświetlanie WebGrid, która obejmuje parametry równa nazwy klas CSS](displaying-data/_static/image20.png)

Aby zobaczyć, co `GetHtml` generowane metody, można sprawdzić źródło strony w przeglądarce. Firma Microsoft nie będzie przejdź do szczegółów w tym miejscu, ale istotną kwestią jest to, że, określając parametry, takie jak `tableStyle`, spowodowane siatki, które mają być Generowanie tagi HTML, jak pokazano poniżej:

`<table class="grid">`

`<table>` Miał znacznik `class` atrybut dodawanych do niego, który odwołuje się do jednej reguły CSS, które wcześniej dodałeś. Ten kod przedstawia podstawowy wzorzec &mdash; różne parametry dla `GetHtml` metody pozwalają możesz odwołanie CSS klasy, aby z nich metoda generuje następnie wraz z kodu znaczników. Co zrobisz z klas CSS zależy od użytkownika.

## <a name="adding-paging"></a>Dodawanie stronicowania

Jako ostatnie zadanie w ramach tego samouczka dodasz stronicowania do siatki. W tej chwili, to żaden problem do wyświetlenia wszystkich filmów na raz. Ale jeśli dodano setki filmów wyświetlania strony będzie długo.

W kodzie strony Zmień wiersz, który tworzy `WebGrid` obiektu z następującym kodem:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Jedyną różnicą między przed jest, że dodano `rowsPerPage` parametr, który jest ustawiony na 3.

Uruchom stronę. Siatka wyświetla 3 wiersze w czasie oraz łączy nawigacji, które umożliwiają przeglądanie filmów w bazie danych:

![Wyświetlanie WebGrid za pomocą stronicowania](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Pojawi się dalej

W następnym samouczku dowiesz się, jak pobrać dane wejściowe użytkownika w postaci za pomocą kodu Razor i C#. Dodasz pole wyszukiwania do strony filmy, aby mógł znaleźć filmów według tytułu lub gatunku.

## <a name="complete-listing-for-movies-page"></a>Kompletna lista strony filmy

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Poprzednie](intro-to-web-pages-programming.md)
> [dalej](form-basics.md)
