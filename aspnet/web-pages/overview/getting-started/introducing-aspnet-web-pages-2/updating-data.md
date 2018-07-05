---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Wprowadzenie do składnika ASP.NET Web Pages — aktualizowanie danych bazy danych | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku dowiesz się, jak zaktualizować wpis (Zmień) istniejącej bazy danych, korzystając z ASP.NET Web Pages (Razor). Przyjęto założenie, że zostały wykonane serii th...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 259034a795b9dff7001165a182bc0e84bb690491
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377061"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Wprowadzenie do wzorca ASP.NET Web Pages — aktualizowanie danych bazy danych
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku dowiesz się, jak zaktualizować wpis (Zmień) istniejącej bazy danych, korzystając z ASP.NET Web Pages (Razor). Przyjęto założenie, że zostały wykonane serii za pośrednictwem [wprowadzanie danych przez przy użyciu formularzy przy użyciu stron ASP.NET Web Pages](entering-data.md).
> 
> Zawartość:
> 
> - Jak wybrać rekord `WebGrid` pomocnika.
> - Jak odczytać pojedynczy rekord z bazy danych.
> - Sposób wstępnego ładowania formularza z wartościami z rekordu bazy danych.
> - Jak zaktualizować istniejący rekord w bazie danych.
> - Sposób przechowywania informacji na stronie bez wyświetlania go.
> - Sposób przechowywania informacji przy użyciu ukrytego pola.
>   
> 
> Funkcje/technologie omówione:
> 
> - `WebGrid` Pomocnika.
> - SQL `Update` polecenia.
> - `Database.Execute` Metody.
> - Ukryte pola (`<input type="hidden">`).


## <a name="what-youll-build"></a>Jakie będziesz tworzyć

W poprzednim samouczku przedstawiono sposób dodawania rekord do bazy danych. Tutaj dowiesz się, jak wyświetlania rekordu do edycji. W *filmy* stronie będą aktualizowane `WebGrid` pomocnika, tak że wyświetla **Edytuj** łącze obok każdego filmu:

![WebGrid wyświetlania, w tym link "Edytuj", dla każdego filmu](updating-data/_static/image1.png)

Po kliknięciu **Edytuj** łącza, spowoduje to przejście do innej strony, której informacje film jest już w postaci:

![Edytuj stronę Film przedstawiający film, aby go edytować](updating-data/_static/image2.png)

Możesz zmienić wartości. Po przesłaniu zmian kodu na stronie aktualizuje bazę danych i umożliwia powrót do listy filmów.

Ta część procesu działa prawie dokładnie tak, jak *AddMovie.cshtml* strony utworzonego w poprzednim samouczku tak wiele części tego samouczka nie będą niczym nowym.

Istnieje kilka sposobów, które można zaimplementować sposobem edytowania filmu indywidualnych. To podejście pokazano została wybrana, ponieważ jest łatwe do wdrożenia i łatwe do zrozumienia.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Dodanie Link edycji do listy filmu

Aby rozpocząć, zaktualizujesz *filmy* stronie, dzięki czemu każdy film również lista zawiera **Edytuj** łącza.

Otwórz *Movies.cshtml* pliku.

W treści strony Zmień `WebGrid` znaczników przez dodanie kolumny. Oto znaczniki zmodyfikowane:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Nowa kolumna jest to:

[!code-html[Main](updating-data/samples/sample2.html)]

Punkt w tej kolumnie jest wyświetlany link (`<a>` elementu) którego tekst jest wyświetlany komunikat "Edytuj". Interesuje nas polega na utworzeniu łącza, który wygląda podobnie do poniższego, po uruchomieniu na stronie, z `id` wartość dla każdego filmu różnią się od:

[!code-css[Main](updating-data/samples/sample3.css)]

Ten link spowoduje wywołanie stronę o nazwie *EditMovie*, i zostaną przetworzone w ciągu zapytania `?id=7` do tej strony.

Składnia służąca do nowej kolumny, która może wyglądać nieco złożone, ale to tylko w przypadku, dlatego umieszcza je ze sobą kilku elementów. Każdego pojedynczego elementu jest bardzo proste. Jeśli możesz skoncentrować się na tylko `<a>` elementu, zobacz ten kod znaczników:

[!code-html[Main](updating-data/samples/sample4.html)]

Podstawowe informacje dotyczące sposobu działania siatki: Siatka wyświetla wiersze, jeden dla każdego rekordu bazy danych i wyświetla kolumny dla każdego pola w rekordzie bazy danych. Podczas każdego wiersza siatki jest budowany, `item` obiekt zawiera rekord bazy danych (element) dla tego wiersza. Taki układ zapewnia sposób w kodzie można pobrać danych dla tego wiersza. Jest to, co widzicie tutaj: wyrażenie `item.ID` otrzymuje wartość Identyfikatora elementu bieżącej bazy danych. Można uzyskać wartości bazy danych (tytuł, gatunku lub rok) tak samo za pomocą `item.Title`, `item.Genre`, lub `item.Year`.

Wyrażenie `"~/EditMovie?id=@item.ID` łączy część zakodowanych docelowy adres URL (`~/EditMovie?id=`) z tym identyfikatorem dynamicznie pochodnych. (Wystąpił `~` operatora w poprzednim samouczku; jest operator programu ASP.NET, który reprezentuje bieżący katalog główny witryny sieci Web.)

Wynik jest, że ta część znaczników w kolumnie po prostu tworzy podobny do niej następujące znaczniki w czasie wykonywania:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Oczywiście rzeczywistej wartości `id` będą inne dla każdego wiersza.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Tworzenie niestandardowego wyświetlania kolumny siatki

Teraz z powrotem do kolumny siatki. Trzy kolumny pierwotnie było w siatce wyświetlane tylko wartości danych (tytuł, gatunku i roku). Na tym ekranie jest określony, przekazując nazwę kolumny bazy danych &mdash; na przykład `grid.Column("Title")`.

Ta nowa **Edytuj** różni się kolumnie łącze. Zamiast określania nazwy kolumny, przechodząc `format` parametru. Ten parametr umożliwia definiowanie znaczników, `WebGrid` pomocnika spowoduje, że wraz z `item` wartości do wyświetlenia danych kolumny jako pogrubiony lub zielonym lub w formacie tego. Na przykład jeśli chce się tytuł pogrubionych, można utworzyć kolumnę, tak jak ten przykład:

[!code-html[Main](updating-data/samples/sample6.html)]

(Różne `@` znaki widoczne w `format` właściwość oznaczyć przejścia między kodzie znaczników oraz wartość kodu.)

Po wiedzieć o `format` właściwości, łatwiej jest zrozumieć, jak nowe **Edytuj** Podsumowując kolumnie łącze:

[!code-html[Main](updating-data/samples/sample7.html)]

Kolumna zawiera *tylko* z kodu znaczników, który renderuje łącze, oraz pewne informacje (ID), są wyodrębniane z rekordu bazy danych dla wiersza.

> [!TIP]
> 
> **Nazwane parametry i parametry pozycyjne dla metody**
> 
> Wiele razy podczas już wywołuje metodę i przekazania parametrów do niego, można po prostu wymieniono wartości parametrów, oddzielonych przecinkami. Poniżej przedstawiono kilka przykładów:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Firma Microsoft nie wspomnieć o ten problem, gdy najpierw widoczny ten kod, ale w każdym przypadku jest przekazywanie parametrów do metod w określonej kolejności &mdash; , kolejność, w której parametry są zdefiniowane w tej metodzie. Aby uzyskać `db.Execute` i `Validation.RequireFields`, jeśli mieszana kolejność wartości, należy przekazać, otrzyma komunikat o błędzie po uruchomieniu na stronie lub co najmniej kilku dziwne wyników. Wyraźnie widać trzeba znać kolejności do przekazania parametrów w. (W programie WebMatrix, IntelliSense mogą pomóc nauczyć się ustalenie nazwa, typ i kolejność parametrów.)
> 
> Jako alternatywę do przekazywania wartości w kolejności, można użyć *nazwanych parametrów*. (Przekazywanie parametrów w kolejności jest znany jako przy użyciu *parametry pozycyjne*.) Dla nazwanych parametrów jawnie dołączysz nazwę parametru podczas przekazywania jego wartość. Wykorzystano nazwanych parametrów już kilka razy w tych samouczkach. Na przykład:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> and
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Nazwane parametry są przydatne w kilku przypadkach, szczególnie w przypadku, gdy metoda pobiera wiele parametrów. Jeden jest, gdy chcesz przekazać tylko jeden lub dwa parametry, ale wartości, które mają być przekazane nie znajdują się wśród pierwszej pozycji na liście parametrów. Inny sytuacja jest, jeśli chcesz kod był bardziej czytelny, przekazując parametry w kolejności najodpowiedniejszy dla Ciebie.
> 
> Oczywiście Aby użyć parametrów nazwanych, trzeba znać nazwy parametrów. Funkcja IntelliSense programu WebMatrix można *Pokaż* nazwy, ale nie można obecnie wypełnieniu je dla Ciebie.


## <a name="creating-the-edit-page"></a>Tworzenie strony edycji

Teraz możesz utworzyć *EditMovie* strony. Gdy użytkownik kliknie **Edytuj** łącza, są one będzie umieszczane na tej stronie.

Utwórz stronę o nazwie *EditMovie.cshtml* i Zamień, co znajduje się w pliku następującym kodem:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Ta znaczników i kodu jest podobne do posiadane przez Ciebie *AddMovie* strony. Istnieje niewielka różnica w tekst dla przycisku Prześlij. Podobnie jak w przypadku *AddMovie* stronie znajduje się `Html.ValidationSummary` wywołania, które będą wyświetlane błędy sprawdzania poprawności, jeśli istnieją. Teraz możemy teraz pomijając wywołania `Validation.Message`na to, że błędy będą wyświetlane w podsumowania weryfikacji. Jak wspomniano w poprzednim samouczku, można użyć podsumowania weryfikacji i komunikaty o błędach poszczególnych w różnych kombinacjach.

Ponownie Zauważ, że `method` atrybutu `<form>` element jest ustawiony na wartość `post`. Podobnie jak w przypadku *AddMovie.cshtml* strony, ta strona sprawia, że zmiany do bazy danych. W związku z tym, należy wykonać ten formularz `POST` operacji. (Aby uzyskać więcej informacji o różnicach między `GET` i `POST` operacji, zobacz [GET, POST i bezpieczeństwa zlecenie HTTP](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) paska bocznego w tym samouczku w formularzach HTML.)

Jak przedstawiono wcześniej samouczka `value` atrybuty pola tekstowe są ustawiane za pomocą kodu Razor w celu wstępnego załadowania ich. Tym razem jednak używasz zmiennych, takich jak `title` i `genre` dla tego zadania, zamiast `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Jak wcześniej, ten kod znaczników będzie wstępnego ładowania wartości pola tekstowe z wartościami filmu. Zostaną wyświetlone za chwilę, dlaczego warto używać zmiennych w tej chwili zamiast `Request` obiektu.

Istnieje również `<input type="hidden">` elementu na tej stronie. Ten element przechowuje identyfikator filmu bez uwidaczniania tego na stronie. Identyfikator jest początkowo przekazane do strony, przy użyciu wartości ciągu zapytania (`?id=7` lub podobne w adresie URL). Ustawiając wartość Identyfikatora w ukrytym polu, możesz sprawdzić, czy jest dostępna po przesłaniu formularza, nawet jeśli nie masz już dostęp do oryginalnego adresu URL strony została wywołana z.

W odróżnieniu od *AddMovie* stronie kod *EditMovie* strona ma dwie odrębne funkcje. Pierwsza funkcja jest fakt, że ta strona jest wyświetlana po raz pierwszy (i *tylko* następnie), ten kod pobiera identyfikator film z ciągu zapytania. Kod, a następnie używa Identyfikatora do odczytu odpowiedniego filmu z bazy danych i wyświetlania (wstępnego ładowania) go w polach tekstowych.

Druga funkcja jest to, że gdy użytkownik kliknie **Prześlij zmiany** przycisku, kod musi odczytać wartości pól tekstowych i sprawdzić ich poprawność. Kod musi zaktualizować element bazy danych z nowymi wartościami. Ta technika jest podobne do dodawania rekord, jak przedstawiono w *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Dodawanie kodu do odczytu pojedynczego filmu

Aby wykonać pierwszej funkcji, Dodaj następujący kod na górze strony:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Większość ten kod znajduje się wewnątrz bloku, który rozpoczyna się `if(!IsPost)`. `!` Operatora "not" oznacza tak oznacza wyrażenie *Jeśli to żądanie nie jest przesyłanie wpis*, czyli pośredni sposób powiedzenie *Jeśli to żądanie po raz pierwszy, uruchamianą na tej stronie*. Jak wspomniano wcześniej, należy uruchomić ten kod *tylko* przy pierwszym uruchomieniu strony. Jeśli nie należy wpisać kod w `if(!IsPost)`, będzie uruchamiany za każdym razem strona, zostanie wywołana, czy po raz pierwszy, lub w odpowiedzi na przycisku kliknij.

Należy zauważyć, że kod zawiera `else` blokowania w tej chwili. Jak już mówiliśmy, kiedy wdrożyliśmy `if` bloków, czasami chcesz uruchomić alternatywnego kodu, jeśli warunek testujesz nie dotyczy. To przypadek, w tym miejscu. Jeśli warunek zostanie spełniony (to znaczy, jeśli identyfikator przekazane do strony jest ok), można odczytać wiersza bazy danych. Jednakże, jeśli warunek nie zakończy się pomyślnie, `else` uruchamia blok i kod ustawia komunikat o błędzie.

## <a name="validating-a-value-passed-to-the-page"></a>Sprawdzanie poprawności wartości przekazane do strony

Kod używa `Request.QueryString["id"]` można pobrać Identyfikatora, który jest przekazywany do strony. Kod sprawdza, czy wartość faktycznie została przekazana dla identyfikatora. Jeśli wartość nie został przekazany, kod ustawia błąd sprawdzania poprawności.

Ten kod zawiera inny sposób, aby zweryfikować informacje. W poprzednim samouczku doświadczenie w pracy z `Validation` pomocnika. Zarejestrowany pola, aby zweryfikować i ASP.NET czy sprawdzanie poprawności i automatycznie wyświetlane błędy przy użyciu `Html.ValidationMessage` i `Html.ValidationSummary`. W tym przypadku jednak użytkownik jest nie tak naprawdę Walidacja danych wejściowych użytkownika. Zamiast tego jest sprawdzanie poprawności wartość, która została przekazana do strony z innego miejsca. `Validation` Pomocnika nie zrobił to.

W związku z tym, możesz sprawdzić wartość samodzielnie, testując go `if(!Request.QueryString["ID"].IsEmpty()`). Jeśli występuje problem, można wyświetlić błąd, używając `Html.ValidationSummary`, tak jak w przypadku `Validation` pomocnika. Aby to zrobić, należy wywołać `Validation.AddFormError` i przekaż go komunikat do wyświetlenia. `Validation.AddFormError` to wbudowana metoda, która pozwala zdefiniować niestandardowe komunikaty, które łączyć się przy użyciu systemu sprawdzania poprawności, które już znasz z. (Później w tym samouczku omówimy sposób wprowadzania ten proces sprawdzania poprawności nieco bardziej niezawodne.)

Po upewnieniu się, że istnieje identyfikator filmu, kod odczytuje bazę danych, Wyszukiwanie elementu pojedynczej bazy danych. (Prawdopodobnie zauważenia ogólny wzorzec operacji bazy danych: otworzyć bazy danych, Zdefiniuj instrukcję SQL i uruchamiania instrukcji.) Tym razem SQL `Select` wyciąg zawiera `WHERE ID = @0`. Identyfikator jest unikatowy, mogą być zwracane tylko jeden rekord.

Zapytanie jest wykonywane przy użyciu `db.QuerySingle` (nie `db.Query`, jak używane dla list filmu), a kod umieszcza wynik w `row` zmiennej. Nazwa `row` dowolnej; zmienne możesz nazwać wszystko chcesz. Zmienne zainicjowana na górze następnie są wypełnione Szczegóły filmu, aby te wartości mogą być wyświetlane w polach tekstowych.

## <a name="testing-the-edit-page-so-far"></a>Testowanie strony edycji (pory)

Jeśli chcesz przetestować stronę, uruchom *filmy* strony teraz, a następnie kliknij przycisk **Edytuj** łącze obok dowolnego filmu. Zobaczysz *EditMovie* strony ze szczegółami wypełnione dla filmu, wybrane:

![Edytuj stronę Film przedstawiający film, aby go edytować](updating-data/_static/image3.png)

Zwróć uwagę, że adres URL strony podobny `?id=10` (lub inny numer). Do tej pory zostały przetestowane, **Edytuj** linki w *filmu* stronie pracy, czy strona jest odczytywania Identyfikatora z ciągu zapytania, i czy baza danych zapytanie o pobranie filmu pojedynczy rekord działa.

Można zmienić informacje o filmach, ale nic się nie dzieje po kliknięciu **Prześlij zmiany**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Dodawanie kodu można zaktualizować film o zmiany wprowadzone przez użytkownika

W *EditMovie.cshtml* plików, do implementowania funkcji second (zapisywanie zmian), Dodaj następujący kod wewnątrz zamykającym nawiasem klamrowym `@` bloku. (Jeśli nie masz pewności, dokładnie, gdzie umieścić kod, możesz obejrzeć [kompletny kod dla strony edytowania filmu](#Complete_Page_Listing_for_EditMovie) , pojawia się na końcu tego samouczka.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Ponownie, ten kod znaczników i kodu jest podobny do kodu w *AddMovie*. Kod znajduje się w `if(IsPost)` zablokować, ponieważ ten kod zadziała tylko wtedy, gdy użytkownik kliknie **Prześlij zmiany** przycisk &mdash; oznacza to, gdy (i tylko wtedy, gdy) wysłał formularza. W tym przypadku nie używasz testów, takich jak `if(IsPost && Validation.IsValid())`— czyli nie łączenia oba testy przy użyciu AND. Na tej stronie, należy najpierw ustalić, czy jest przesłanie formularza (`if(IsPost)`) i dopiero wtedy zarejestrować pola do sprawdzania poprawności. Następnie będzie można przetestować wyniki weryfikacji (`if(Validation.IsValid()`). Przepływ są nieco inne niż w *AddMovie.cshtml* strony, ale efekt jest taki sam.

Pobierz wartości pól tekstowych przy użyciu `Request.Form["title"]` i podobny kod dla siebie `<input>` elementów. Zwróć uwagę, w tym momencie kod pobiera identyfikator filmu poza pole ukryte (`<input type="hidden">`). Jeśli strona został uruchomiony po raz pierwszy, kod stało się identyfikator z ciągu zapytania. Możesz pobrać wartości z ukryte pole, aby upewnić się, że otrzymujesz identyfikator filmu, który pierwotnie został wyświetlony w przypadku, gdy ciąg zapytania jakiś sposób został zmieniony od tego momentu.

Bardzo ważna różnica między *AddMovie* kod i ten kod jest w tym kodzie SQL `Update` instrukcji zamiast `Insert Into` instrukcji. Poniższy przykład pokazuje składnię SQL `Update` instrukcji:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Można określić wszystkie kolumny w dowolnej kolejności, a nie zawsze konieczne zaktualizowanie wszystkich kolumn podczas `Update` operacji. (Nie można zaktualizować Identyfikatora, ponieważ, będzie obowiązywać Zapisz rekord jako nowy rekord i nie jest dozwolona dla `Update` operacji.)

> [!NOTE] 
> 
> **Ważne** `Where` klauzuli o identyfikatorze to bardzo ważne, ponieważ jest to, jak baza danych zna bazę danych, która rekordów, które chcesz zaktualizować. Jeśli została przerwana `Where` klauzuli bazy danych będzie aktualizował *co* rekordów w bazie danych. W większości przypadków, które byłyby awarii.


W kodzie wartości, aby zaktualizować są przekazywane do instrukcji SQL przy użyciu symboli zastępczych. Powtarzaj co już powiedzieliśmy przed: ze względów bezpieczeństwa *tylko* używać symbole zastępcze do przekazywania wartości do instrukcji SQL.

Po kod używa `db.Execute` do uruchomienia `Update` instrukcji, zostanie przekierowany do strony, gdzie można zobaczyć zmiany.

> [!TIP] 
> 
> **Instrukcje SQL różnych, różnych metod**
> 
> Być może Zauważyłeś, można używać nieco inne metody do uruchamiania różnych instrukcji języka SQL. Aby uruchomić `Select` zapytanie to potencjalnie zwraca wiele rekordów, należy użyć `Query` metody. Aby uruchomić `Select` zapytanie, które znasz będzie zwracać tylko jeden element bazy danych, należy użyć `QuerySingle` metody. Aby uruchamiać polecenia, wprowadzić zmiany, ale które nie zwracają elementy bazy danych, należy użyć `Execute` metody.
> 
> Musisz mieć różnych metod, ponieważ każda z nich zwraca różne wyniki, co już różnic między `Query` i `QuerySingle`. ( `Execute` Metoda również faktycznie zwraca wartość &mdash; , liczba wierszy do bazy danych, które miały wpływ polecenia &mdash; , ale możesz już został zostanie zignorowany, do tej pory.)
> 
> Oczywiście `Query` metoda może zwracać tylko jeden wiersz bazy danych. Jednak ASP.NET zawsze traktuje wyniki `Query` metodę jako kolekcję. Nawet jeśli metoda ta zwraca tylko jeden wiersz, należy wyodrębnić ten pojedynczy wiersz z kolekcji. W związku z tym, w sytuacjach, w którym możesz *znać* wrócisz tylko jeden wiersz, jest nieco bardziej wygodne do użycia `QuerySingle`.
> 
> Dostępnych jest kilka metod, które wykonują określone typy operacji bazy danych. Możesz znaleźć listę metod bazy danych w [interfejsu API stron sieci Web platformy ASP.NET — dokumentacja szybkie](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Tworzenie niezawodnych sprawdzania poprawności dla innych identyfikator

Przy pierwszym uruchomieniu strony, można uzyskać identyfikator filmu od ciągu zapytania, aby przejść, Pobierz ten film z bazy danych. Możesz upewnienie się, że faktycznie nie ma wartości przejść, sprawdź, czy przy użyciu tego kodu:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Ten kod może zostać użyty do upewnij się, że jeśli użytkownik otrzyma do *EditMovies* strony bez zaznaczania filmu w *filmy* strony, strony zostanie wyświetlony komunikat o błędzie przyjazny dla użytkownika. (W przeciwnym razie użytkownicy będą Zobacz błąd, który będzie prawdopodobnie tylko mylić ich).

Jednak ta weryfikacja nie jest bardzo niezawodne. Strony może być również wywoływane w tych błędów:

- Identyfikator nie jest liczbą. Na przykład strony można wywoływanej przy użyciu adresu URL, takich jak `http://localhost:nnnnn/EditMovie?id=abc`.
- Identyfikator jest liczbą, ale odwołuje się do filmów, która nie istnieje (na przykład `http://localhost:nnnnn/EditMovie?id=100934`).

Jeśli zastanawiasz się wyświetlić błędy, które wynikają z tych adresów URL, uruchom *filmy* strony. Wybierz opcję film, aby edytować, a następnie zmień adres URL *EditMovie* strony do adresu URL, który zawiera alfabetyczne, identyfikator lub identyfikator filmu nie istnieje.

Dlatego co należy zrobić? Pierwszy poprawka jest upewnij się, że nie tylko jest identyfikator przekazywany do strony, ale że identyfikator jest liczbą całkowitą. Zmień kod dla `!IsPost` testu wyglądać następująco:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Po dodaniu drugi warunek do `IsEmpty` testów, powiązanej z `&&` (operator logiczny oraz):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Być może pamiętasz z [wprowadzenie do programowania programu ASP.NET Web Pages](../introducing-razor-syntax-c.md) samouczek, w którym metod, takich jak `AsBool` `AsInt` konwertują ciąg znaków do innych typów danych. `IsInt` — Metoda (i inne, takie jak `IsBool` i `IsDateTime`) są podobne. Jednak one tylko do celów testowych czy możesz *można* przekonwertować ciągu, bez rzeczywistego wykonania konwersji. Tutaj możesz jest zasadniczo informujący o tym *Jeśli można przekonwertować na liczbę całkowitą wartość ciągu zapytania...* .

Potencjalny problem jest wyszukiwanie filmów, która nie istnieje. Kod, aby uzyskać filmu wygląda podobnie do tego kodu:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

W przypadku przekazania `movieId` wartość `QuerySingle` metody, która nie jest zgodny z rzeczywistych filmu, nic nie zostanie zwrócone i instrukcje, postępuj zgodnie z (na przykład `title=row.Title`) powodować błędy.

Ponownie istnieje łatwo to naprawić. Jeśli `db.QuerySingle` metody nie zwróciło żadnych wyników `row` zmienna będzie miała wartość null. Dzięki czemu można sprawdzić, czy `row` zmienna ma wartość null, przed podjęciem próby uzyskania wartości z niego. Poniższy kod dodaje `if` bloku wokół instrukcji, które pobierają wartości z `row` obiektu:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Za pomocą tych dwóch dodatkowe testy weryfikacji strony staje się bardziej punktor dowodu. Kompletny kod dla `!IsPost` gałęzi wygląda teraz następująco:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Możemy zauważyć, że raz, to zadanie jest dobrze wykorzystane do `else` bloku. Jeśli testy nie zakończy się pomyślnie, `else` bloki Ustaw komunikaty o błędach.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Dodanie Linku, aby wrócić do strony filmy

Szczegóły ostateczne i przydatne jest dodać link do *filmy* strony. W zwykłych przepływu zdarzeń, użytkownicy zostaną uruchomione w *filmy* strony, a następnie kliknij przycisk **Edytuj** łącza. Które oferuje je *EditMovie* strony, w którym można edytować film i kliknij przycisk. Po kodzie przetwarza zmiany, zostanie przekierowany do *filmy* strony.

Jednak:

- Użytkownik może zrezygnować z wprowadzić zmiany.
- Użytkownik może stają się coraz do tej strony, nie klikając pierwszy **Edytuj** łącze w *filmy* strony.

W obu przypadkach chcesz ułatwić ich powrócić do listy głównego. Go łatwo to naprawić &mdash; Dodaj następujący kod zaraz po zamykającym `</form>` tagu w znaczniku:

[!code-html[Main](updating-data/samples/sample19.html)]

Ten kod znaczników korzysta z tej samej składni dla `<a>` element, który w tym samouczku gdzie indziej. Adres URL zawiera `~` oznacza "root witryny sieci Web".

## <a name="testing-the-movie-update-process"></a>Testowanie procesu aktualizacji filmu

Teraz możesz przetestować. Uruchom *filmy* strony, a następnie kliknij przycisk **Edytuj** obok filmu. Gdy *EditMovie* zostanie wyświetlona strona, wprowadzić zmiany do filmów i kliknij przycisk **Prześlij zmiany**. Gdy zostanie wyświetlona na liście filmów, upewnij się, że zmiany są wyświetlane.

Aby upewnić się, czy działa sprawdzania poprawności, kliknij przycisk **Edytuj** dla innego filmu. Po przejściu do *EditMovie* strony, wyczyść **gatunku** pola (lub **roku** pola i / lub), a następnie spróbuj przesłać wprowadzone zmiany. Zostanie wyświetlony błąd, jak można oczekiwać:

![Edytuj stronę Film przedstawiający błędy sprawdzania poprawności](updating-data/_static/image4.png)

Kliknij przycisk **powrócić do listy filmów** łącze, aby porzucić zmiany i powrócić do *filmy* strony.

## <a name="coming-up-next"></a>Pojawi się dalej

W następnym samouczku pokazano, jak można usunąć rekordu filmu.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Kompletna lista strony filmu (aktualizowane wraz z łączami do edycji)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Kompletna lista strony dla strony filmu edycji

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania dla sieci Web platformy ASP.NET przy użyciu składni Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrukcja UPDATE SQL](http://www.w3schools.com/sql/sql_update.asp) witrynie W3Schools

> [!div class="step-by-step"]
> [Poprzednie](entering-data.md)
> [dalej](deleting-data.md)
