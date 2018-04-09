---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Wprowadzenie do składnika ASP.NET Web Pages — aktualizowanie bazy danych | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten samouczek pokazuje, jak zaktualizować wpis (Zmień) istniejącej bazy danych, korzystając z stron sieci Web platformy ASP.NET (Razor). Przyjęto założenie, że zostały wykonane serii th...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: e889cd27e2267a08f7b6ea708c92e35edbdd7a1a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Wprowadzenie do strony sieci Web ASP.NET — aktualizowanie bazy danych
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek pokazuje, jak zaktualizować wpis (Zmień) istniejącej bazy danych, korzystając z stron sieci Web platformy ASP.NET (Razor). Przyjęto założenie, że zostały wykonane serii za pomocą [wprowadzanie danych przez za pomocą formularzy przy użyciu stron ASP.NET Web Pages](entering-data.md).
> 
> Zawartość:
> 
> - Jak wybrać rekord `WebGrid` pomocnika.
> - Jak można odczytać pojedynczego rekordu z bazy danych.
> - Jak wstępnie formularza z wartości rekordu bazy danych.
> - Jak zaktualizować istniejący rekord w bazie danych.
> - Sposób przechowywania informacji na stronie bez ich wyświetlania.
> - Jak używać do przechowywania informacji o ukryte pole.
>   
> 
> Funkcje/technologie omówione:
> 
> - `WebGrid` Pomocnika.
> - SQL `Update` polecenia.
> - `Database.Execute` Metody.
> - Ukryte pola (`<input type="hidden">`).


## <a name="what-youll-build"></a>Jakie będzie kompilacji

W poprzednich samouczka przedstawiono sposób dodać rekordu do bazy danych. W tym miejscu dowiesz się, jak można wyświetlić rekordu do edycji. W *filmy* strony, będzie aktualizowana `WebGrid` Pomocnik, którego nie wyświetla **Edytuj** link obok każdego filmu:

![Wyświetl WebGrid tym łącze "Edytuj" dla każdego filmu](updating-data/_static/image1.png)

Po kliknięciu **Edytuj** łącza, spowoduje to przejście do innej strony, gdy informacje filmu są już w postaci:

![Edytuj stronę Film przedstawiający filmu do edycji](updating-data/_static/image2.png)

Można zmienić wartości. Po przesłaniu zmian kodu na stronie aktualizuje bazę danych i powrót do listy filmów.

Ta część procesu prawie dokładnie tak samo, jak działa *AddMovie.cshtml* utworzonego w poprzedniej samouczek, dlatego większość tego samouczka należy zapoznać się strony.

Istnieje kilka metod, które można zaimplementować sposób edytowania indywidualne filmów. Podejście pokazano została wybrana, ponieważ jest łatwa do wdrożenia i łatwy do zrozumienia.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Dodawanie do listy filmów łącza edycji

Aby rozpocząć, będą aktualizowane *filmy* strony, tak aby zawierała każdego filmu również wyświetlanie **Edytuj** łącza.

Otwórz *Movies.cshtml* pliku.

W treści strony Zmień `WebGrid` znaczników przez dodanie kolumny. Oto modyfikacji kod znaczników:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Nowa kolumna jest ta:

[!code-html[Main](updating-data/samples/sample2.html)]

Punkt w tej kolumnie jest do wyświetlenia łącza (`<a>` element) którego tekst mówi "Edytuj". Interesuje nas jest utworzenie łącza, który wygląda jak poniżej, po uruchomieniu na stronie, z `id` wartość różne dla każdego filmu:

[!code-css[Main](updating-data/samples/sample3.css)]

To łącze wywoła stronę o nazwie *EditMovie*, i zostanie przekazany ciąg zapytania `?id=7` do tej strony.

Składnia dla nowej kolumny, która może wyglądać nieco złożonego, ale jest to tylko, ponieważ umieszcza je ze sobą kilka elementów. Każdy element poszczególnych jest prosta. Jeśli możesz skoncentrować się na właśnie `<a>` elementu, zobacz ten kod znaczników:

[!code-html[Main](updating-data/samples/sample4.html)]

Pewną wiedzę na temat działania siatki: Siatka wyświetla wiersze, po jednej dla każdego rekordu bazy danych, i wyświetla kolumny dla każdego pola w rekordzie bazy danych. Podczas każdego wiersza siatki jest tworzona, `item` obiektu zawiera rekord bazy danych (element) dla tego wiersza. To rozmieszczenie umożliwia kod w celu pobrania danych dla tego wiersza. Jest widoczna w tym miejscu: wyrażenie `item.ID` otrzymuje wartość Identyfikatora bieżącego elementu bazy danych. Można uzyskać wartości bazy danych (tytuł, genre lub rok) taki sam sposób jak przy użyciu `item.Title`, `item.Genre`, lub `item.Year`.

Wyrażenie `"~/EditMovie?id=@item.ID` łączy część ustalony docelowy adres URL (`~/EditMovie?id=`) z tym identyfikatorem dynamicznie pochodnych. (Widać `~` operatora w poprzednim samouczek; jest operatora programu ASP.NET, który reprezentuje bieżącego katalogu głównego witryny sieci Web.)

Wynik jest, że ta część znacznika w kolumnie po prostu tworzy przypominać następujący kod w czasie wykonywania:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Oczywiście rzeczywistej wartości `id` będą inne dla każdego wiersza.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Tworzenie niestandardowego wyświetlania kolumny siatki

Teraz z powrotem do kolumn siatki. Trzy kolumny trzeba pierwotnie było w siatce wyświetlane tylko wartości danych (tytuł, genre i rok). Ten ekran jest określony przez nazwa kolumny bazy danych &mdash; na przykład `grid.Column("Title")`.

Nowy **Edytuj** kolumny łącza jest inna. Zamiast określania nazwy kolumny, przechodząc `format` parametru. Ten parametr umożliwia definiowanie znaczników który `WebGrid` pomocnika spowoduje, że wraz z programem `item` wartość do wyświetlania danych kolumny jako pogrubiony lub zielony lub w dowolnym formacie tego. Na przykład jeśli potrzebujesz tytuł, aby wyświetlone pogrubioną można utworzyć kolumnę, tak jak ten przykład:

[!code-html[Main](updating-data/samples/sample6.html)]

(Różne `@` znaki widoczne w `format` właściwości oznaczyć przejścia między znaczników i wartość kodu.)

Po wiedzieć o `format` właściwości, łatwiej zrozumieć, jak nowy **Edytuj** Podsumowując kolumny łącza:

[!code-html[Main](updating-data/samples/sample7.html)]

Kolumna zawiera *tylko* znaczników, który renderuje łącze, oraz niektóre informacje (ID) który został wyodrębniony rekordu bazy danych dla wiersza.

> [!TIP]
> 
> **Parametrów nazwanych i pozycyjnych parametrów metody**
> 
> Wiele razy po utworzeniu wywołuje metodę i przekazania parametrów do niego, po prostu zawartych wartości parametrów oddzielonych przecinkami. Oto kilka przykładów:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Firma Microsoft nie wspomina problem, gdy najpierw był wyświetlany ten kod, ale w każdym przypadku jest przekazywanie parametrów do metod w określonej kolejności &mdash; mianowicie, kolejność, w której parametry są zdefiniowane w tej metodzie. Aby uzyskać `db.Execute` i `Validation.RequireFields`, jeśli zamienione wartości należy przekazać, otrzyma komunikat o błędzie po uruchomieniu na stronie lub co najmniej pewnych dziwne wyników. Wyraźnie widać trzeba znać do przekazania parametrów w kolejności. (W programie WebMatrix, IntelliSense ułatwiają ustalenie potrzebnej nazwę, typ i kolejność parametrów.)
> 
> Zamiast wartości przekazywanie w kolejności, można użyć *parametrów nazwanych*. (Przekazywanie parametrów w kolejności nazywa się przy użyciu *parametrów pozycyjnych*.) Dla parametrów nazwanych jawnie uwzględniono nazwę parametru podczas przekazywania jego wartość. Używano nazwane parametry już kilka razy w tych samouczkach. Na przykład:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> and
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Parametry nazwane jest skorzystać z kilku sytuacjach, szczególnie w przypadku, gdy metoda pobiera dużo parametrów. Jest jeden, gdy chcesz przekazać tylko jeden lub dwa parametry, ale wśród pierwszej pozycji na liście parametrów nie są wartości, które chcesz przekazać. Innej sytuacji jest, aby kod był bardziej czytelny przekazując parametry w kolejności priorytetów dla Ciebie.
> 
> Oczywiście Aby użyć parametrów nazwanych, trzeba znać nazwy parametrów. Program WebMatrix IntelliSense może *Pokaż* nazwy, ale nie można obecnie wypełnieniu je automatycznie.


## <a name="creating-the-edit-page"></a>Tworzenie edycji strony

Teraz możesz utworzyć *EditMovie* strony. Po kliknięciu **Edytuj** łącza, są one będą umieszczane na tej stronie.

Utwórz stronę o nazwie *EditMovie.cshtml* i Zastąp nowości w pliku o następujący kod:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Ten kod znaczników i kodu jest podobny do masz *AddMovie* strony. Istnieje niewielka różnica w tekst dla przycisku Prześlij. Jak *AddMovie* strony, jest `Html.ValidationSummary` wywołaniu, które będą wyświetlane błędy sprawdzania poprawności, jeśli istnieją. Teraz możemy Cię pomijając wywołania `Validation.Message`, ponieważ błędy będą wyświetlane w podsumowania weryfikacji. Jak wspomniano w poprzedniej samouczek, można użyć podsumowania weryfikacji i komunikaty o poszczególnych w różnych kombinacjach.

Zwróć uwagę, ponownie że `method` atrybutu `<form>` element jest ustawiony na wartość `post`. Jak *AddMovie.cshtml* strony, strony dokonuje zmian w bazie danych. W związku z tym należy wykonać ten formularz `POST` operacji. (Aby uzyskać więcej informacji o różnicach między `GET` i `POST` operacji, zobacz [GET, POST i bezpieczeństwa zlecenie HTTP](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) paska bocznego w samouczku w formularzach HTML.)

Jak przedstawiono wcześniej samouczka `value` atrybutów pola tekstowe są ustawiane z kodu Razor w celu wstępnego załadowania je. Teraz, jednak używasz zmiennych, na przykład `title` i `genre` dla tego zadania, a nie `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Jako przed, ten kod znaczników zostaną wstępnie wartości pole tekstowe z wartościami filmu. Pojawi się za chwilę, dlaczego warto używać zmiennych w tej chwili zamiast `Request` obiektu.

Istnieje również `<input type="hidden">` elementu na tej stronie. Ten element przechowuje identyfikator filmu bez staje się widoczny na stronie. Identyfikator jest początkowo przekazany do strony przy przy użyciu wartości ciągu zapytania (`?id=7` lub podobne w adresie URL). Ustawiając wartość Identyfikatora w ukrytym polu, można upewnij się, że jest dostępny po przesłaniu formularza, nawet jeśli nie masz już dostępu do strony została wywołana z oryginalnego adresu URL.

W odróżnieniu od *AddMovie* strony kod *EditMovie* strony ma dwa różne funkcje. Pierwszej funkcji jest fakt, że ta strona jest wyświetlana po raz pierwszy (i *tylko* następnie), kod pobiera identyfikator film z ciągu zapytania. Kod, a następnie używa Identyfikatora do odczytywania odpowiedniego film z bazy danych programu i wyświetlić (wstępnie) go w polach tekstowych.

Druga funkcja występuje wtedy, gdy użytkownik kliknie **Prześlij zmiany** przycisku kod ma odczytać wartości pola tekstowe i sprawdzania ich poprawności. Ten kod musi zaktualizować element bazy danych przy użyciu nowych wartości. Ta technika jest podobne do dodawania rekordów, jak przedstawiono w *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Dodawanie kodu do odczytu pojedynczego filmu

Aby wykonać pierwszej funkcji, Dodaj ten kod do górnej części strony:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

Większość ten kod znajduje się wewnątrz bloku, która rozpoczyna się `if(!IsPost)`. `!` Operatora "not" oznacza, że oznacza wyrażenie *Jeśli to żądanie nie jest przesyłanie post*, która jest pośredni sposób z informacją o tym *Jeśli to żądanie jest ta strona została uruchomiona po raz pierwszy*. Jak wspomniano wcześniej, należy uruchamiać ten kod *tylko* przy pierwszym uruchomieniu strony. Jeśli nie umieść kod w `if(!IsPost)`, czy uruchamiać za każdym razem strony, zostanie wywołany, czy po raz pierwszy, lub w odpowiedzi na przycisku kliknij.

Należy zauważyć, że kod zawiera `else` blokowania w tej chwili. Jak możemy powiedzieć, gdy wprowadzono `if` bloków, czasami chcesz uruchomić kod alternatywnego, jeśli testowany warunek nie jest spełniony. To sytuacji, w tym miejscu. Jeśli warunek przekazuje (jeśli przekazany do strony identyfikator jest prawidłowy), przeczytanie wiersz z bazy danych. Jednak jeśli warunek nie przeszło, `else` uruchamia blok i kod ustawia komunikat o błędzie.

## <a name="validating-a-value-passed-to-the-page"></a>Sprawdzanie poprawności wartości przekazane do strony

W kodzie użyto `Request.QueryString["id"]` można uzyskać Identyfikatora, który jest przekazywany do strony. Kod upewnia się, że wartość rzeczywiście została przekazana dla identyfikatora. Jeśli wartość nie została przekazana, kod ustawia błąd sprawdzania poprawności.

Ten kod zawiera inną metodę weryfikacji informacji. W samouczku poprzednie doświadczenie z `Validation` pomocnika. Rejestrowane pola do sprawdzania poprawności i ASP.NET została sprawdzania poprawności i automatycznie wyświetlone błędy przy użyciu `Html.ValidationMessage` i `Html.ValidationSummary`. W takim przypadku jednak możesz jest nie naprawdę sprawdzanie poprawności danych wejściowych użytkownika. Zamiast tego jest sprawdzanie poprawności wartość, która została przekazana do strony z innej lokalizacji. `Validation` Pomocnika nie pracę za użytkownika.

Należy więc wybrać wartość samodzielnie, testując go przy użyciu `if(!Request.QueryString["ID"].IsEmpty()`). Jeśli występuje problem, można wyświetlić błąd, za pomocą `Html.ValidationSummary`, jak w przypadku `Validation` pomocnika. Aby to zrobić, należy wywołać `Validation.AddFormError` i przekaż go komunikat do wyświetlenia. `Validation.AddFormError` to wbudowana metoda, która pozwala zdefiniować niestandardowe wiadomości, które wiąże się przy użyciu systemu sprawdzania poprawności, które znasz już. (W dalszej części tego samouczka będziesz omawianiu jak ten proces sprawdzania poprawności nieco bardziej niezawodne.)

Po upewnieniu się, że jest Identyfikatorem filmu, kod odczytuje bazę danych, Wyszukiwanie elementu pojedynczej bazy danych. (Prawdopodobnie zauważenia ogólne wzorca operacji bazy danych: otworzyć bazy danych, Zdefiniuj instrukcję SQL i uruchom instrukcję.) Teraz, SQL `Select` instrukcja zawiera `WHERE ID = @0`. Identyfikator jest unikatowy, mogą być zwracane tylko jeden rekord.

Zapytanie jest wykonywane przy użyciu `db.QuerySingle` (nie `db.Query`, jak użyć listy film), i kod umieszcza wynik w `row` zmiennej. Nazwa `row` dowolnej; można określić nazwę zmienne niczego chcesz. Zmienne zainicjować u góry są wypełnione ze szczegółami film, aby te wartości mogą być wyświetlane w polach tekstowych.

## <a name="testing-the-edit-page-so-far"></a>Testowanie edycji strony (wykonanej do tej pory)

Jeśli chcesz testować stronę, uruchom *filmy* teraz i kliknij przycisk **Edytuj** link obok żadnych filmu. Zobaczysz *EditMovie* strony Szczegóły wypełnione dla filmu wybrano:

![Edytuj stronę Film przedstawiający filmu do edycji](updating-data/_static/image3.png)

Zwróć uwagę, że adres URL strony zawiera przypominać `?id=10` (lub inny numer). Do tej pory przetestowaniu który **Edytuj** łączy w *film* strony pracy, czy strony odczytuje identyfikator z ciągu zapytania i że bazy danych zapytania można pobrać filmu pojedynczego rekordu działa.

Można zmienić informacje film, ale nic się nie dzieje po kliknięciu **Prześlij zmiany**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Dodawanie kodu do zaktualizowania film z zmian wprowadzonych przez użytkownika

W *EditMovie.cshtml* plików, aby zaimplementować funkcji second (zapisywanie zmian), Dodaj następujący kod wewnątrz zamykającym `@` bloku. (Jeśli nie masz pewności, dokładnie, gdzie umieścić kod, można przyjrzeć się [kompletny kod dla strony Edytuj film](#Complete_Page_Listing_for_EditMovie) który pojawia się na końcu tego samouczka.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Ponownie, jest podobny do kodu, w tym znaczników i kodu *AddMovie*. Kod znajduje się w `if(IsPost)` zablokować, ponieważ ten kod działa tylko wtedy, gdy użytkownik kliknie **Prześlij zmiany** przycisku &mdash; oznacza to, kiedy (i tylko wtedy, gdy) wysłał formularza. W takim przypadku nie używasz testu, takie jak `if(IsPost && Validation.IsValid())`— to znaczy nie łączenia zarówno testy przy użyciu AND. Na tej stronie, należy najpierw określić, czy jest przesłanie formularza (`if(IsPost)`), a następnie zarejestruj pól do sprawdzenia poprawności. Następnie można przetestować wyniki sprawdzania poprawności (`if(Validation.IsValid()`). Przepływ są nieco inne niż w *AddMovie.cshtml* strony, ale efekt jest taki sam.

Pobiera wartości pól tekstowych przy użyciu `Request.Form["title"]` i podobny kod dla siebie `<input>` elementów. Zwróć uwagę, tym razem kod pobiera identyfikator filmu ukryte pola (`<input type="hidden">`). Po stronie pierwszego uruchomienia, kod uzyskano identyfikator poza ciągu zapytania. Pobiera wartość z pola ukrytego upewnij się, że jesteś już identyfikator film, który pierwotnie został wyświetlony, jeśli ciąg zapytania jakiś sposób został zmieniony od tego momentu.

Naprawdę istotną różnicą między *AddMovie* kodu i ten kod jest w tym kodzie wykorzystania SQL `Update` instrukcji zamiast `Insert Into` instrukcji. W poniższym przykładzie przedstawiono składnię SQL `Update` instrukcji:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Można określić kolumn w dowolnej kolejności i niekoniecznie nie trzeba zaktualizować każdej kolumny podczas `Update` operacji. (Nie można zaktualizować Identyfikatora, ponieważ który będzie obowiązywać zapisać rekord jako nowego rekordu, a nie jest dozwolona dla `Update` operacji.)

> [!NOTE] 
> 
> **Ważne** `Where` klauzuli o identyfikatorze jest bardzo ważne, ponieważ jest to, jak bazy danych zna bazę danych, która rekordów, które chcesz zaktualizować. Jeśli została pozostawiona `Where` klauzuli bazy danych będzie aktualizował *co* rekordów w bazie danych. W większości przypadków, które będą awarii.


W kodzie można zaktualizować wartości są przekazywane do instrukcji SQL przy użyciu symboli zastępczych. Powtarzaj, co zostało możemy powiedzieć przed: ze względów bezpieczeństwa *tylko* użycia symbole zastępcze w celu przekazania wartości do instrukcji SQL.

Po kodzie użyto `db.Execute` do uruchomienia `Update` instrukcji przekierowuje wróć do strony listy, w którym można zobaczyć zmiany.

> [!TIP] 
> 
> **Instrukcje SQL różnych, różnych metod**
> 
> Zwróć uwagę, czy nieco inne metody używane do uruchamiania różnych instrukcji SQL. Aby uruchomić `Select` zapytanie to potencjalnie zwraca wiele rekordów, możesz użyć `Query` metody. Aby uruchomić `Select` zapytania, który zwróci tylko jeden element bazy danych, możesz użyć `QuerySingle` metody. Aby uruchomić polecenia, który wprowadzić zmiany, ale które nie zwracają elementy bazy danych, należy użyć `Execute` metody.
> 
> Musisz mieć różne metody, ponieważ każdy z nich zwraca różne wyniki, jak przedstawiono już różnic między `Query` i `QuerySingle`. ( `Execute` Metoda również faktycznie zwraca wartość &mdash; mianowicie, liczbę wierszy bazy danych, które miały wpływ polecenia &mdash; , ale można już został ignorowanie który wykonanej do tej pory.)
> 
> Oczywiście `Query` metoda może zwracać tylko jeden wiersz bazy danych. Jednak ASP.NET zawsze traktuje wyniki `Query` metody jako kolekcja. Nawet wtedy, gdy metoda zwróci wartość tylko jeden wiersz, należy wyodrębnić ten pojedynczy wiersz z kolekcji. W związku z tym w sytuacjach, w którym możesz *wiedzieć* zostanie wyświetlony ponownie tylko jeden wiersz, jest bardziej wygodne bit `QuerySingle`.
> 
> Istnieje kilka metod, wykonujących określone typy operacji w bazie danych. Można znaleźć listę metod bazy danych w [interfejsu API stron sieci Web platformy ASP.NET — dokumentacja szybkie](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Tworzenie weryfikacji dla Identyfikatora więcej niezawodne

Po raz pierwszy uruchamia strony, otrzymasz identyfikator film z ciągu zapytania, dzięki czemu możesz pobrać ten film z bazy danych. Wprowadzone należy się upewnić, że faktycznie wystąpił wartość przejść, sprawdź, czy przy użyciu tego kodu:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Ten kod jest używany do upewnij się, że jeśli użytkownik pobiera do *EditMovies* strony bez zaznaczania filmu w *filmy* strony, strona będzie wyświetlana przyjazna dla użytkownika komunikat. (W przeciwnym razie użytkownicy czy Zobacz błąd, który będzie prawdopodobnie właśnie mylić je).

Jednak stopniu niezawodną nie tej weryfikacji. Strony może również być wywoływane z tych błędów:

- Identyfikator nie jest liczbą. Na przykład strony można wywołać przy użyciu adresu URL, takie jak `http://localhost:nnnnn/EditMovie?id=abc`.
- Identyfikator jest liczbą, lecz przywołuje film, który nie istnieje (na przykład `http://localhost:nnnnn/EditMovie?id=100934`).

Jeśli Cię to ciekawi wyświetlić błędy wynikające z tych adresów URL, uruchom *filmy* strony. Wybierz opcję film, aby edytować, a następnie zmień adres URL *EditMovie* strony do adresu URL, który zawiera alfabetyczną identyfikator lub identyfikator filmu nie istnieje.

Dlatego co należy zrobić? Pierwsze rozwiązanie polega na upewnij się, że nie tylko jest identyfikator przekazywany do strony, ale czy identyfikator jest liczbą całkowitą. Zmień kod `!IsPost` testu, aby wyglądały jak w tym przykładzie:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Dodano drugi warunek do `IsEmpty` testu, połączony z `&&` (logiczne i):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Pamiętasz z [wprowadzenie do programowania stron sieci Web ASP.NET](../introducing-razor-syntax-c.md) samouczka dotyczącego metod, takich jak `AsBool` `AsInt` przekonwertować ciągu znaków na inny typ danych. `IsInt` — Metoda (i innych osób, takich jak `IsBool` i `IsDateTime`) są podobne. Jednak sprawdzają one tylko czy możesz *można* przekonwertować ciągu, bez rzeczywistego wykonania konwersji. Dlatego w tym miejscu możesz jest zasadniczo informujący o tym *Jeśli można przekonwertować na liczbę całkowitą wartość ciągu kwerendy...* .

Potencjalny problem szuka film, który nie istnieje. Kod w celu uzyskania filmu wygląda ten kod:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

W przypadku przekazania `movieId` do wartości `QuerySingle` metodę, która nie jest zgodny z rzeczywistego film, nic nie zostanie zwrócone i instrukcje wykonaj (na przykład `title=row.Title`) spowodować błędy.

Ponownie jest łatwe. Jeśli `db.QuerySingle` — metoda nie zwróciło żadnych wyników `row` zmienna będzie mieć wartość null. Dlatego można sprawdzić, czy `row` zmienna ma wartość null, zanim spróbujesz uzyskać wartości z niego. Poniższy kod dodaje `if` bloku wokół instrukcji, które pobiera wartości z `row` obiektu:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Z tych dwóch testów weryfikacji dodatkowe strony staje się bardziej punktor potwierdzenia. Kompletny kod dla `!IsPost` gałęzi teraz wygląda następująco:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Firma Microsoft będzie należy ponownie to zadanie dobre wykorzystanie dla `else` bloku. Jeśli nie, `else` bloki ustawić komunikaty o błędach.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Dodanie łącz, aby powrócić do strony filmy

Szczegóły ostateczne i przydatne jest aby dodać łącze do *filmy* strony. W zwykłym przepływu zdarzeń, użytkownicy zostaną uruchomione w *filmy* i kliknij przycisk **Edytuj** łącza. Który wprowadzono do *EditMovie* strony, w którym można edytować filmu i kliknij przycisk. Po kodzie przetwarza zmiany, przekierowuje do *filmy* strony.

Jednak:

- Użytkownik może zrezygnować z wprowadzanie zmian.
- Użytkownik może stają się coraz do tej strony bez najpierw kliknij **Edytuj** łącze w *filmy* strony.

W obu przypadkach należy ułatwiają je, aby powrócić do listy głównego. Jest łatwy poprawka &mdash; Dodaj następujący kod znaczników zaraz po upływie `</form>` tag w kod znaczników:

[!code-html[Main](updating-data/samples/sample19.html)]

Ten kod znaczników używa tej samej składni `<a>` element, który w tym samouczku w innym miejscu. Adres URL zawiera `~` oznacza "root witryny sieci Web".

## <a name="testing-the-movie-update-process"></a>Testowanie procesu aktualizacji film

Teraz możesz przetestować. Uruchom *filmy* , a następnie kliknij przycisk **Edytuj** obok filmu. Gdy *EditMovie* zostanie wyświetlona strona, wprowadzić zmiany do filmu i kliknij przycisk **Prześlij zmiany**. Zostanie wyświetlona na liście film, upewnij się, że zmiany są pokazywane.

Aby upewnić się, czy działa sprawdzania poprawności, kliknij przycisk **Edytuj** dla innego filmu. Po przejściu do *EditMovie* wyczyść **Genre** pola (lub **roku** pola lub oba), a następnie spróbuj przesłać zmiany. Zostanie wyświetlony błąd, jak można oczekiwać:

![Edytuj stronę Film przedstawiający błędy sprawdzania poprawności](updating-data/_static/image4.png)

Kliknij przycisk **powrócić do listy filmów** łącze, aby odrzucić zmiany i wróć do *filmy* strony.

## <a name="coming-up-next"></a>Powtarzający się dalej

W następnym samouczku pojawi się, jak usunąć rekord filmu.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Pełna lista strony Movie (aktualizowane wraz z łączami Edycja)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Zakończenie listę strony dla strony filmu edycji

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrukcji SQL UPDATE](http://www.w3schools.com/sql/sql_update.asp) w witrynie W3Schools

> [!div class="step-by-step"]
> [Poprzednie](entering-data.md)
> [dalej](deleting-data.md)
