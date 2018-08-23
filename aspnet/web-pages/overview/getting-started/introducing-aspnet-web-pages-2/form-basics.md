---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Przedstawiamy ASP.NET Web Pages — podstawy formularzy HTML | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku przedstawiono podstawowe informacje dotyczące sposobu tworzenia formularza wejściowego i sposób obsługi danych wejściowych użytkownika, gdy używasz stron ASP.NET Web Pages (Razor). A teraz, jak...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 5cf628b63e868e3f76aa19879d4af554241979c0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754708"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Wprowadzenie do wzorca ASP.NET Web Pages — podstawy formularzy HTML
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku przedstawiono podstawowe informacje dotyczące sposobu tworzenia formularza wejściowego i sposób obsługi danych wejściowych użytkownika, gdy używasz stron ASP.NET Web Pages (Razor). A Skoro masz bazę danych, użyjesz umiejętności związane z formularza, aby umożliwić użytkownikom znajdowanie filmy określonych w bazie danych. Przyjęto założenie, że zostały wykonane serii za pośrednictwem [wprowadzenie do wyświetlania danych przy użyciu stron ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Zawartość:
> 
> - Jak utworzyć formularz za pomocą standardowych elementów HTML.
> - Jak odczytać użytkownika wejściowych w formularzu.
> - Jak utworzyć zapytanie SQL, aby selektywnie uzyska dane przy użyciu wyszukiwania termin, użytkownik dostarcza.
> - Jak mają pola na stronie "Pamiętaj" użytkownik wprowadzi.
>   
> 
> Funkcje/technologie omówione:
> 
> - `Request` Obiektu.
> - SQL `Where` klauzuli.


## <a name="what-youll-build"></a>Jakie będziesz tworzyć

W poprzednim samouczku utworzono bazę danych, do niego dodana danych i następnie użyć `WebGrid` element pomocniczy służący do wyświetlania danych. W tym samouczku dodasz pole wyszukiwania umożliwiające znajdowanie filmy określonego rodzaju lub których tytuł zawiera niezależnie od programu word, możesz wprowadzić. (Na przykład można znaleźć wszystkie filmy, których gatunek "Action", lub których tytuł zawiera "Harry" lub "Adventure.")

Po wykonaniu tych czynności z tego samouczka będziesz mieć strony podobny do poniższego:

![Strona filmów z wyszukiwania gatunku i tytuł](form-basics/_static/image1.png)

Lista części strony jest taka sama, jak ostatni samouczek &mdash; siatki. Różnica będzie, że siatki będzie widoczny tylko filmy, wyszukiwanie.

## <a name="about-html-forms"></a>Informacje o formularzach HTML

(Jeśli masz doświadczenie w tworzeniu formularzy HTML i różnica między `GET` i `POST`, możesz pominąć tę sekcję.)

Formularz zawiera elementów wejściowych użytkownika &mdash; pola tekstowe, przyciski, przyciski radiowe, pola wyboru, listy rozwijane i tak dalej. Użytkownicy wypełnić te kontrolki lub dokonanie wyboru i Prześlij formularz przez kliknięcie przycisku.

Podstawowa składnia HTML formularza jest przedstawionymi w tym przykładzie:

[!code-html[Main](form-basics/samples/sample1.html)]

Po uruchomieniu na stronie znajduje się w tym znaczników tworzy prosty formularz, który wygląda jak na tej ilustracji:

![Podstawowe formularza HTML jako renderowany w przeglądarce](form-basics/_static/image2.png)

`<form>` Element zawiera elementy HTML do przesłania. (Łatwe błędu, aby upewnić się dodać elementy do strony, ale następnie zapomnij umieść je wewnątrz `<form>` elementu. W tym przypadku nic nie jest przesyłany.) `method` Atrybut informuje przeglądarkę, jak przesłać dane wejściowe użytkownika. Ustaw tę opcję na `post` przeprowadzania aktualizacji na serwerze lub do `get` Jeśli masz tylko pobieranie danych z serwera.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST i bezpieczeństwa czasownik HTTP**
> 
> Protokół używany przez przeglądarki i serwery do wymiany informacji, protokołu HTTP jest niezwykle prosty w jej podstawowe operacje. Przeglądarki używają tylko kilka poleceń na wysyłanie żądań do serwerów. Podczas pisania kodu dla sieci web, warto zrozumieć te zleceń i jak przeglądarką i serwerem z nich korzystać. I daleko zleceń najczęściej używane są te:
> 
> - `GET`. Przeglądarka używa tego żądania można pobrać elementu z serwera. Na przykład po wpisaniu adresu URL w przeglądarce wykonuje przeglądarka `GET` operację żądania żądanej strony. Jeśli strona zawiera grafiki, dodatkowe wykonuje przeglądarka `GET` operacje, aby pobrać obrazy. Jeśli `GET` operacja ma do przekazywania informacji do serwera, informacje jest przekazywany jako część adresu URL w ciągu zapytania.
> - `POST`. Przeglądarka wysyła `POST` żądania w celu przesyłania danych, które mają być dodane lub zmienione na serwerze. Na przykład `POST` zlecenia służy do tworzenia rekordów w bazie danych lub zmiana istniejących zasad. W większości przypadków, gdy wypełnienia formularza i kliknij przycisk Prześlij, przeglądarka wykonuje `POST` operacji. W `POST` operacji, dane są przekazywane do serwera znajduje się w treści strony.
> 
> Na istotną różnicę między tych poleceń jest to, że `GET` operacji nie powinien zmian na serwerze — i umieść go w sposób nieco bardziej abstrakcyjnego `GET` operacja powoduje zmianę stanu na serwerze. Można wykonać `GET` operacji na tych samych zasobów co tyle razy, ile chcesz, a nie należy zmieniać tych zasobów. (A `GET` operacji często mówi się jako "bezpieczne" lub użyć termin techniczny jest *idempotentne*.) Z drugiej strony, oczywiście `POST` żądania zmiany wystąpił na serwerze każdym razem po wykonaniu operacji.
> 
> Dwa przykłady będą ilustrujących tę różnicę. Podczas wyszukiwania przy użyciu aparatu, takiej jak Bing czy Google, wypełnij formularz, który składa się z jednego pola tekstowego, a następnie kliknij przycisk wyszukiwania. Wykonuje przeglądarka `GET` operacji o wartości wprowadzone w polu przekazanych jako część adresu URL. Za pomocą `GET` operacji dla tego typu formularza jest dobrym rozwiązaniem, ponieważ operacja wyszukiwania nie zmienia żadnych zasobów na serwerze, po prostu pobiera informacje.
> 
> Teraz należy wziąć pod uwagę procesu zamawiania coś w trybie online. Wypełnij szczegóły zamówienia, a następnie kliknij przycisk Prześlij. Ta operacja zostanie `POST` żądania, ponieważ operacja spowoduje zmiany na serwerze, na przykład nowy rekord zlecenia, zmiany w danych konta i może być wiele innych zmian. W odróżnieniu od `GET` operacji nie można powtórzyć swoje `POST` żądania — Jeśli tak, nie, zawsze możesz ponownie przesłane żądanie, wygeneruje nowe zamówienie na serwerze. (W takich sytuacjach witryn sieci Web będzie często ostrzega użytkownika, aby nie więcej niż jeden raz kliknij przycisk Prześlij lub spowoduje wyłączenie przycisku Prześlij, dzięki czemu użytkownik nie ponownie Prześlij formularz przypadkowo.)
> 
> W ramach tego samouczka użyjesz zarówno `GET` operacji i `POST` operacji do pracy z formularzy HTML. Wyjaśnimy, w każdym przypadku Dlaczego zlecenie, którego używasz jest jeden z nich.
> 
> (Aby dowiedzieć się więcej na temat zleceń HTTP, zobacz [definicje metod](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artykuł w witrynie W3C.)


Większość elementów wejściowych użytkownika są HTML `<input>` elementów. Wyglądają jak `<input type="type" name="name">,` gdzie *typu* wskazuje rodzaj kontrolki danych wejściowych użytkownika ma. Typowe z nich są to następujące elementy:

- Pole tekstowe: `<input type="text">`
- Pole wyboru: `<input type="check">`
- Przycisk radiowy: `<input type="radio">`
- Przycisk: `<input type="button">`
- Przycisk Prześlij: `<input type="submit">`

Można również użyć `<textarea>` elementu do utworzenia wielowierszowego pola tekstowego i `<select>` elementu do utworzenia listy rozwijanej lub przewijaną listę. (Aby uzyskać więcej informacji na temat HTML tworzą elementy, zobacz [formularzy HTML i dane wejściowe](http://www.w3schools.com/html/html_forms.asp) witrynie W3Schools.)

`name` Atrybut jest bardzo ważne, ponieważ nazwa jest sposób uzyskasz wartość elementu w dalszej części, ponieważ pojawi się wkrótce.

Interesujące części jest Deweloper musisz wykonać strony, czy przy użyciu danych wprowadzonych przez użytkownika. Nie ma żadnych wbudowanym zachowaniu powiązane z tymi elementami. Zamiast tego należy pobrać wartości, które użytkownik ma wprowadzane lub wybierane oraz zrobić coś z nimi. To, co dowiesz się, w tym samouczku.

> [!TIP] 
> 
> **HTML5 i formularze**
> 
> Być może wiesz, HTML, jest w stanie przejścia i najnowszej wersji (HTML5) obejmuje obsługę bardziej intuicyjne dla użytkowników wprowadzić informacje. Na przykład przy użyciu kodu HTML5, (Deweloper strony) można określić strony ma datę wprowadzenia przez użytkownika. Przeglądarka następnie można wyświetlić automatycznie kalendarza, a nie użytkownik musi ręcznie wprowadź datę. Jednak HTML5 nowego i nie jest obsługiwana we wszystkich przeglądarkach na jeszcze.
> 
> Strony ASP.NET Web Pages obsługuje protokół HTML5, danych wejściowych w zakresie, w jakim jest przeglądarki użytkownika. Dla pomysł nowych atrybutów dla `<input>` elementu przy użyciu kodu HTML5, zobacz [HTML &lt;wejściowych&gt; typ atrybutu](http://www.w3schools.com/html/html_form_input_types.asp) witrynie W3Schools.


## <a name="creating-the-form"></a>Tworzenie formularza

W programie WebMatrix w **pliki** obszaru roboczego, otwórz *Movies.cshtml* strony.

Po zamykającym `</h1>` tagu i przed otwarciem `<div>` tag `grid.GetHtml` wywołania, Dodaj następujący kod:

[!code-html[Main](form-basics/samples/sample2.html)]

Ten kod znaczników tworzy formularz zawierający pole tekstowe o nazwie `searchGenre` i przycisk Prześlij. Tekst pole, a następnie prześlij przycisk są ujęte w `<form>` elementu którego `method` ma ustawioną wartość atrybutu `get`. (Należy pamiętać, że jeśli nie umieścić pole tekstowe i przycisk wewnątrz Prześlij `<form>` elementu, nic nie zostanie przesłany, po kliknięciu przycisku.) Możesz użyć `GET` zlecenie tutaj ponieważ tworzysz formularza, nie wprowadzać żadnych zmian na serwerze — po prostu powoduje wyszukiwania. (W poprzednim samouczku użyto `post` metoda, która jest, jak przesłać zmian na serwer. Zobaczysz, w następnym samouczku ponownie.)

Uruchom stronę. Mimo że jeszcze nie zdefiniowano żadnych zachowanie formularza, zostanie wyświetlony, wygląda następująco:

![Strona filmów z pola wyszukiwania dla gatunku](form-basics/_static/image3.png)

Wprowadź wartość w polu tekstowym, takich jak "Komedia." Następnie kliknij przycisk **gatunku wyszukiwania**.

Zanotuj adres URL strony. Ponieważ ustawisz `<form>` elementu `method` atrybutu `get`, wprowadzona wartość jest teraz częścią programu ciągu zapytania w adresie URL, w następujący sposób:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Odczytywanie wartości z formularza

Strona zawiera już kodu, który pobiera dane z bazy danych i wyświetla wyniki w siatce. Teraz należy dodać kod odczytujący wartość w polu tekstowym, aby można było uruchomić zapytanie SQL, które zawierają termin wyszukiwania.

Ponieważ metoda formularza jest ustawiona na `get`, można odczytać wartość, która została podana w polu tekstowym za pomocą kodu, podobnie do poniższego:

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString` Obiektu ( `QueryString` właściwość `Request` obiektu) zawiera wartości elementów, które zostały przesłane w ramach `GET` operacji. `Request.QueryString` Właściwość zawiera *kolekcji* (lista) wartości, które są przesyłane w formularzu. Aby uzyskać poszczególne wartości, należy określić nazwę elementu, który chcesz. Dlatego musisz mieć `name` atrybutu na `<input>` — element (`searchTerm`) który tworzy pole tekstowe. (Aby uzyskać więcej informacji na temat `Request` obiektu, zobacz [paska bocznego](#BKMK_TheRequestObject) później.)

To proste, można odczytać wartości pola tekstowego. Jednak jeśli użytkownik nie wpisywać nic w ogóle w polu tekstowym, ale kliknięto **wyszukiwania** mimo to, możesz zignorować kliknij ten przycisk, ponieważ nie ma nic do wyszukiwania.

Poniższy kod jest przykładem, który pokazuje, jak zaimplementować te warunki. (Nie trzeba dodać ten kod jeszcze; należy to zrobić za chwilę).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Test dzieli w ten sposób:

- Pobierz wartość `Request.QueryString["searchGenre"]`, czyli wartość wprowadzonego w `<input>` elementu o nazwie `searchGenre`.
- Dowiedz się, czy jest pusta przy użyciu `IsEmpty` metody. Ta metoda jest standardowym sposobem ustalenia, czy coś (na przykład element formularza) zawiera wartość. Ale naprawdę istotne tylko wtedy, gdy ma ona *nie* pusty, w związku z tym...
- Dodaj `!` operatora przed `IsEmpty` testu. ( `!` Operator oznacza logiczne NOT).

W prostych słowach całą `if` warunek przekłada się na następujących: *Jeśli element searchGenre formularza nie jest pusta, następnie...*

Ten blok doświadczonych Trwa tworzenie zapytania, który używa termin wyszukiwania. Należy to zrobić w następnej sekcji.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Obiekt żądania**
> 
> `Request` Obiekt zawiera wszystkie informacje wysyłane przez przeglądarkę do aplikacji po żądane lub przesłane strony. Ten obiekt zawiera wszystkie informacje, które użytkownik udostępnia wartości w oknie tekstowym lub plik do przekazania. Zawiera także szeroką gamę dodatkowych informacji, takich jak pliki cookie, wartości w ciągu zapytania adresu URL (jeśli istnieje), ścieżka pliku strony, która jest uruchomiona, typ przeglądarki, która używa użytkownika, listę języków, które są ustawione w przeglądarce i wiele więcej.
> 
> `Request` Obiekt jest *kolekcji* (lista) z wartości. Otrzymasz poszczególnych wartości z kolekcji określając jej nazwę:
> 
> `var someValue = Request["name"];`
> 
> `Request` Obiekt faktycznie udostępnia kilka podzbiory. Na przykład:
> 
> - `Request.Form` zapewnia wartości z elementy wewnątrz przesłane `<form>` elementu, jeśli żądanie jest `POST` żądania.
> - `Request.QueryString` podano tylko wartości ciągu zapytania adresu URL. (W adresie URL, takich jak `http://mysite/myapp/page?searchGenre=action&page=2`, `?searchGenre=action&page=2` części adresu URL jest ciągiem zapytania.)
> - `Request.Cookies` Kolekcja umożliwia dostęp do plików cookie wysłanych przez przeglądarkę.
> 
> Można pobrać wartości, który znasz przesłanego formularza, możesz użyć `Request["name"]`. Alternatywnie, można użyć bardziej szczegółowych wersji `Request.Form["name"]` (dla `POST` żądania) lub `Request.QueryString["name"]` (dla `GET` żądań). Oczywiście *nazwa* jest nazwa elementu, który można pobrać.
> 
> Nazwa elementu, który chcesz pobrać musi być unikatowa w obrębie kolekcji, którego używasz. Dlatego `Request` obiekt zawiera pliki, takie jak `Request.Form` i `Request.QueryString`. Załóżmy, że strona zawiera element formularza o nazwie `userName` i *również* zawiera plik cookie o nazwie `userName`. Jeśli otrzymasz `Request["userName"]`, jest niejednoznaczne mają wartości formularza lub plik cookie. Jednak jeśli `Request.Form["userName"]` lub `Request.Cookie["userName"]`, jest on o wartości, które można pobrać.
> 
> Jest dobrą praktyką określonych oraz używanie podzbiór `Request` interesującego Cię, takich jak `Request.Form` lub `Request.QueryString`. Dla prostych stron, które tworzysz w ramach tego samouczka to prawdopodobnie nie naprawdę że różnic. Jednak podczas tworzenia bardziej złożonych stron za pomocą jawnego wersji `Request.Form` lub `Request.QueryString` może pomóc uniknąć problemów, które mogą wystąpić, gdy strona zawiera formularza (lub wiele form), plików cookie, wartości ciągu zapytania i tak dalej.


## <a name="creating-a-query-by-using-a-search-term"></a>Trwa tworzenie zapytania przy użyciu wyszukiwany termin

Teraz gdy wiesz, jak uzyskać wyszukiwany termin, wprowadzonej przez użytkownika, można utworzyć kwerendę, która go używa. Należy pamiętać, że aby pobrać wszystkie elementy film z bazy danych, używasz zapytania SQL, który wygląda jak ta instrukcja:

`SELECT * FROM Movies`

Aby uzyskać tylko niektóre filmy, trzeba użyć zapytania, które obejmuje `Where` klauzuli. Ta klauzula umożliwia określenie warunków, w której wiersze są zwracane przez zapytanie. Oto przykład:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Podstawowy format jest `WHERE column = value`. Możesz użyć różnych operatorów, oprócz po prostu `=`, takiej jak `>` (większe niż) `<` (mniejsze niż), `<>` (nie równa się), `<=` (mniejsze niż lub równe), itd., w zależności od tego, czego szukasz.

W przypadku, gdy zastanawiasz się, instrukcji SQL nie jest uwzględniana &mdash; `SELECT` jest taka sama jak `Select` (lub nawet `select`). Jednak osoby często wielką literą słów kluczowych w instrukcji SQL, takie jak `SELECT` i `WHERE`, aby ułatwić interpretowanie.

### <a name="passing-the-search-term-as-a-parameter"></a>Przekazując termin wyszukiwania jako parametr

Wyszukiwanie określonego rodzaju jest dość proste (`WHERE Genre = 'Action'`), ale chcesz mieć możliwość wyszukiwania dowolnego rodzaju, wprowadzonych przez użytkownika. Aby to zrobić, Utwórz jako zapytanie SQL, która zawiera symbol zastępczy dla wartości do wyszukania. To polecenie będzie wyglądać:

`SELECT * FROM Movies WHERE Genre = @0`

Symbol zastępczy jest `@` znak następuje zero. Jak może przypuszczalnie zapytania może zawierać wiele symboli zastępczych i będzie nosić `@0`, `@1`, `@2`itp.

Ustawienie kwerendy i faktycznie przekazywanie ich wartości, możesz użyć kodu podobne do następującego:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Ten kod jest podobny do jakie czynności zostały już wykonane do wyświetlania danych w siatce. Tylko różnice są następujące:

- Zapytanie zawiera symbol zastępczy (`WHERE Genre = @0"`).
- Zapytania są umieszczane w zmiennej (`selectCommand`); przed przekazywana bezpośrednio do kwerendy `db.Query` metody.
- Gdy wywołujesz `db.Query` metody przekażesz zapytania i wartość symbolu zastępczego. (Jeśli zapytanie ma wiele symboli zastępczych, należy wprowadzić je wszystkie jako osobne wartości do metody.)

Jeśli wszystkie te elementy są umieszczone razem, otrzymasz następujący kod:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Ważne!** Przy użyciu symboli zastępczych (takich jak `@0`) w celu przekazania wartości do polecenia SQL jest *bardzo ważne* zabezpieczeń. Sposób, zostanie wyświetlony w tym miejscu przy użyciu symboli zastępczych dla danych zmiennej jest jedynym sposobem, należy konstruować poleceń SQL.
> 
> Nigdy nie Skonstruuj instrukcję SQL, umieszczając ze sobą, tekst dosłowny (złączenie) i wartości, który jest pobierany z użytkownika. Łączenie danych wejściowych użytkownika do instrukcji SQL spowoduje otwarcie witryny *ataku polegającego na iniekcji SQL* gdzie złośliwy użytkownik przesyła wartości do strony, które hack bazy danych. (Możesz dowiedzieć się więcej w artykule [wstrzyknięcie kodu SQL](https://msdn.microsoft.com/library/ms161953.aspx) witryny sieci Web MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Aktualizowanie strony filmów z kodem wyszukiwania

Teraz możesz zaktualizować kod w *Movies.cshtml* pliku. Aby rozpocząć, Zastąp kod w bloku kodu, w górnej części strony, przy użyciu tego kodu:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Różnica polega na tym, została utworzona, zapytanie w `selectCommand` zmienną, która będzie przekazać do `db.Query` później. Umieszczenie instrukcję SQL do zmiennej pozwala zmienić instrukcję, która jest wykonasz przeprowadza się wyszukiwanie.

Po usunięciu również te dwa wiersze, w których użytkownik zostanie przywrócona do stanu później:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Nie chcesz jeszcze uruchomione zapytanie (oznacza to, że wywołanie `db.Query`) i nie chcesz zainicjować `WebGrid` pomocnika jeszcze jednej. Odbywa się na powyższych czynności po zapewnieniu instrukcję SQL, która musi zostać uruchomione.

Po tym nowych bloku można dodać nowej logiki obsługę wyszukiwania. Kompletny kod będzie wyglądać następująco. Zaktualizuj kod na stronie tak, aby odpowiadała następująco:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Strona działa teraz następująco. Za każdym razem, gdy strona działa kod zostanie otwarty w bazie danych i `selectCommand` zmienna jest ustawiona na instrukcję SQL, która pobiera wszystkie rekordy z `Movies` tabeli. Inicjuje również kod `searchTerm` zmiennej.

Jednak jeśli bieżące żądanie zawiera wartość dla `searchGenre` element, ustawia kod `selectCommand` do innego zapytania — to znaczy, na taki, który zawiera `Where` klauzuli do wyszukiwania określonego rodzaju. Ustawia również `searchTerm` do niezależnie od rodzaju została przekazana do pola wyszukiwania (co może mieć wartości nothing).

Niezależnie od tego, które SQL Instrukcja znajduje się w `selectCommand`, następnie wywołuje kod `db.Query` Aby uruchomić zapytanie, podając mu dowolną znajduje się w `searchTerm`. Jeśli nie ma nic `searchTerm`, nie ma znaczenia, ponieważ w takim przypadku nie ma żadnego parametru, aby przekazać wartość do `selectCommand` mimo to.

Na koniec kod inicjalizuje `WebGrid` pomocnika za pomocą wyników zapytania, podobnie jak wcześniej.

Możesz zobaczyć, że poprzez umieszczenie instrukcji SQL i wyszukiwany termin do zmiennych, zostały dodane elastyczność w kodzie. Jak zobaczysz później w tym samouczku, można użyć tego podstawowe struktury i dodawaj logikę różnego rodzaju wyszukiwania.

## <a name="testing-the-search-by-genre-feature"></a>Testowanie funkcji wyszukiwania według gatunku

W programie WebMatrix, uruchom *Movies.cshtml* strony. Zostanie wyświetlona strona pole tekstowe dla gatunku.

Wprowadź gatunku, które zostały wprowadzone w jednej z Twoich rekordów testów, następnie kliknij przycisk **wyszukiwania**. Teraz wyświetlić listę właśnie filmy, które odpowiadają tym gatunku:

![Stronę filmy, po zakończeniu wyszukiwania dla gatunku "Comedies"](form-basics/_static/image4.png)

Wprowadź inny gatunku i wyszukać ponownie. Spróbuj wprowadzić gatunek przy użyciu wszystkie małe lub wielkie litery, dzięki czemu można zobaczyć, że wyszukiwanie nie jest uwzględniana wielkość liter.

## <a name="remembering-what-the-user-entered"></a>"Zapamiętywanie" użytkownik wprowadzi

Zwróć uwagę, że po wprowadzono określonego rodzaju i kliknięciu **gatunku wyszukiwania**, widoczny na liście w tego rodzaju. Jednakże, w polu tekstowym wyszukiwania był pusty &mdash; innymi słowy, nie pamiętać, co użytkownik wprowadził.

Należy zrozumieć, dlaczego to zachowanie. Po przesłaniu strony przeglądarce wysyła żądanie do serwera sieci web. Gdy ASP.NET odbiera żądanie, tworzy wystąpienie dobór strony, uruchamia kod w jego i ponownie powoduje wyświetlenie strony do przeglądarki. W efekcie jednak strony nie wie, że po zakończeniu pracy z poprzednią wersją samego siebie. Wszystkie wie, że jest jej Otrzymano żądanie, w której niektóre dane formularza w nim.

Za każdym razem, gdy żądania o stronę &mdash; czy po raz pierwszy lub po jego przesłaniu &mdash; otrzymujesz nowej strony. Serwer sieci web ma Brak pamięci ostatniego żądania. Ani nie ASP.NET, a żaden z nich nie jest przeglądarki. Tylko połączenie między te wystąpienia oddzielne strony to wszystkie dane przesyłane między nimi. Po przesłaniu strony, na przykład nowe wystąpienie strony można uzyskać danych formularza, który została wysłana przez wcześniejsze wystąpienie. (Można przekazać dane między stronami w inny sposób i jest używanie plików cookie).

Posiadanie sposobu na opisanie tej sytuacji jest powiedzieć, że strony sieci web są *bezstanowe*. Serwery sieci Web i strony oraz elementy na stronie nie utrzymują żadnych informacji na temat stanu poprzedniej strony. Sieci web został zaprojektowany w ten sposób, ponieważ zachowaniem stanu dla poszczególnych żądań może szybko wyczerpaniu zasobów serwerów sieci web, które często może obsługiwać tysięcy nawet setki tysięcy żądań na sekundę.

Dlatego pole tekstowe jest pusta. Po przesłaniu strony ASP.NET utworzyć nowe wystąpienie klasy strony i został uruchomiony przy użyciu kodu i znaczników. Nie wykryto w tym kodzie, który informację, ASP.NET, aby umieścić wartość w polu tekstowym. Dlatego ASP.NET nie podejmować żadnych działań, a pole tekstowe był renderowany bez wartości w nim.

Brak faktycznie prosty sposób obejść ten problem. Gatunku, wprowadzony w polu tekstowym *jest* udostępniane użytkownikowi w kodzie &mdash; znajduje się w `Request.QueryString["searchGenre"]`.

Aktualizowanie kodu znaczników dla pola tekstowego tak, aby `value` wartość pochodzi atrybutu `searchTerm`, w tym przykładzie, takich jak:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Na tej stronie można także ustawiono `value` atrybutu `searchTerm` zmiennej, ponieważ zmienna zawiera także gatunek wprowadzony. Jednak przy użyciu `Request` obiektu, aby ustawić `value` atrybutu, jak pokazano w tym miejscu jest standardowym sposobem wykonania tego zadania. (Zakładając, że chcesz to zrobić nawet &mdash; w niektórych sytuacjach możesz chcieć renderowania strony *bez* wartości w polach. To wszystko zależy co się dzieje z aplikacją).

> [!NOTE]
> Nie można "pamiętasz" wartość pola tekstowego, które jest używane hasło. Byłoby lukę w zabezpieczeniach mogli wypełnić pole hasła przy użyciu kodu.


Ponownie uruchom strony, wprowadź określonego rodzaju, a następnie kliknij przycisk **gatunku wyszukiwania**. Teraz nie tylko zobaczysz wyniki wyszukiwania, ale pole tekstowe pamięta, co wprowadzony czas ostatniego:

![Strona, na którym widać, że pole tekstowe jest "zapamiętanych" poprzedni wpis](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Wyszukiwanie dowolny wyraz w tytule

Możesz teraz wyszukać dowolnego rodzaju, ale można także wyszukać tytuł. Trudno jest uzyskać tytuł PRAWDA podczas wyszukiwania, więc zamiast tego możesz wyszukać słowo, które pojawia się w dowolnym miejscu wewnątrz tytuł. Aby to zrobić w języku SQL, użyj `LIKE` operatorów i składni podobnie do następującego:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

To polecenie pobiera wszystkie filmy o tytułach zawiera "adventure". Kiedy używasz `LIKE` operatora i zawierać znaki wieloznaczne `%` jako część termin wyszukiwania. Wyszukiwanie `LIKE 'adventure%'` oznacza "począwszy od"adventure"". (Z technicznego punktu widzenia, oznacza to, "string"adventure", po której następuje niczego.") Podobnie, wyszukiwany termin `LIKE '%adventure'` oznacza, że "wszystko, czym następuje ciąg"adventure"", czyli powiedzieć "kończącą"adventure"" w inny sposób.

Wyszukiwany termin `LIKE '%adventure%'` w związku z tym oznacza, że "z"adventure"dowolne miejsce w tytule." (Z technicznego punktu widzenia "wszystko w tytule, następuje"adventure", następuje niczego.")

Wewnątrz `<form>` elementu, Dodaj następujący kod bezpośrednio w zamykającym `</div>` tag w przypadku wyszukiwania gatunku (tuż przed zamykającym `</form>` elementu):

[!code-html[Main](form-basics/samples/sample10.html)]

Kod służący do obsługi to wyszukiwanie jest podobny do kodu wyszukiwania gatunku, z tą różnicą, że trzeba zrobić `LIKE` wyszukiwania. Wewnątrz bloku kodu, w górnej części strony, dodaj to `if` block tuż za `if` blok wyszukiwania gatunku:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Ten kod używa tej samej logiki był wyświetlany poprzednio, z tą różnicą, że używa wyszukiwania `LIKE` operatora i umieszcza kod "`%`" przed i po nim termin wyszukiwania.

Zwróć uwagę, jak było łatwe do dodania innych kryteriów wyszukiwania do strony. Wszystko, co trzeba było robić to:

- Utwórz `if` blok, który przetestowane, aby zobaczyć, czy pole wyszukiwania odpowiednich miał wartość.
- Ustaw `selectCommand` zmienną instrukcję SQL.
- Ustaw `searchTerm` zmienną wartość do przekazania do zapytania.

Poniżej przedstawiono bloku kompletny kod, który zawiera logikę nowe wyszukiwanie wg tytułu:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Poniżej przedstawiono podsumowanie ten kod wykonuje:

- Zmienne `searchTerm` i `selectCommand` są inicjowane na górze. Zamierzasz ustawić te zmienne na odpowiedni termin (jeśli istnieje) i odpowiednie polecenie SQL oparte na stronie działanie użytkownika. Domyślne wyszukiwanie jest prostym przypadku pobierania wszystkich filmów z bazy danych.
- W testach dla `searchGenre` i `searchTitle`, zestawów kodu `searchTerm` wartość ma być wyszukiwany. Te bloki kodu również ustawić `selectCommand` na odpowiednie polecenie SQL dla tego wyszukiwania.
- `db.Query` Metoda jest wywoływana tylko raz za pomocą dowolnego polecenia SQL znajduje się w `selectedCommand` i każda wartość znajduje się w `searchTerm`. W przypadku żadnego terminu wyszukiwania (nie gatunku i programu word nie tytułu), wartość `searchTerm` jest ciągiem pustym. Jednakże, nie ma znaczenia, ponieważ w takim przypadku zapytanie nie wymaga parametru.

## <a name="testing-the-title-search-feature"></a>Testowanie funkcji wyszukiwania tytułu

Teraz można przetestować strony ukończone wyszukiwania. Uruchom *Movies.cshtml*.

Wprowadź określonego rodzaju, a następnie kliknij przycisk **gatunku wyszukiwania**. Siatka wyświetla filmy tego rodzaju, takich jak przed.

Wybierz wyraz, który tytuł, a następnie kliknij przycisk **Przeszukaj tytuły**. Siatka wyświetla filmów, które mają ten wyraz w tytule.

![Po zakończeniu wyszukiwania dla "" w tytule stronę filmy](form-basics/_static/image6.png)

Oba pola tekstowe jest puste, a następnie kliknij przycisk albo. Siatka wyświetla wszystkie filmy.

## <a name="combining-the-queries"></a>Łączenie zapytań

Może się okazać, czy wyłączne wyszukiwania, które można wykonywać. Tytuł i gatunek nie może wyszukać w tym samym czasie, nawet jeśli oba pola wyszukiwania ma ich wartości. Nie można na przykład wyszukaj wszystkie filmy akcji, których tytuł zawiera "Adventure". (Jak strony kanonicznej zakodowanej teraz, po wprowadzeniu wartości dla tytułu i gatunku, wyszukiwanie wg tytułu pobiera priorytet). Do tworzenia wyszukiwania, który łączy warunków, będzie trzeba utworzyć zapytanie SQL, które ma składnię, jak pokazano poniżej:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

I trzeba uruchomić zapytanie przy użyciu instrukcji, podobnie do następującej (po):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Tworzenie logiki, aby zezwolić na wiele kombinacji kryteriów wyszukiwania można uzyskać nieco zaangażowani, jak widać. W związku z tym będziemy już w tym miejscu.

## <a name="coming-up-next"></a>Pojawi się dalej

W następnym samouczku utworzysz stronę, która używa formularza, aby umożliwić użytkownikom dodawać filmy w bazie danych.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Kompletna lista strony filmu (zaktualizowane za pomocą wyszukiwania)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Klauzuli SQL WHERE](http://www.w3schools.com/sql/sql_where.asp) witrynie W3Schools
- [Definicje metod](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artykuł w witrynie W3C

> [!div class="step-by-step"]
> [Poprzednie](displaying-data.md)
> [dalej](entering-data.md)
