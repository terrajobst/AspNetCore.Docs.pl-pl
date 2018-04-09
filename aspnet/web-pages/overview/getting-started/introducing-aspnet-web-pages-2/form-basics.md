---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Wprowadzenie do składnika ASP.NET Web Pages — podstawowe informacje z formularza HTML | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym samouczku przedstawiono podstawowe informacje dotyczące sposobu tworzenia formularza danych wejściowych i sposób obsługi danych wejściowych użytkownika, gdy używasz stron sieci Web platformy ASP.NET (Razor). I teraz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 6f44f74774c2fa6338524987779e15f3940d1830
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Wprowadzenie do strony sieci Web ASP.NET - podstawy formularza HTML
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym samouczku przedstawiono podstawowe informacje dotyczące sposobu tworzenia formularza danych wejściowych i sposób obsługi danych wejściowych użytkownika, gdy używasz stron sieci Web platformy ASP.NET (Razor). A Skoro masz już bazę danych, użyjesz swoje umiejętności formularza, aby umożliwić użytkownikom znajdowanie filmy określonych w bazie danych. Przyjęto założenie, że zostały wykonane serii za pomocą [wprowadzenie do wyświetlania danych przy użyciu stron ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Zawartość:
> 
> - Jak utworzyć formularz przy użyciu standardowych elementów HTML.
> - Jak odczytywać użytkownika wejściowych w postaci.
> - Jak utworzyć zapytanie SQL, aby selektywnie pobiera dane przy użyciu wyszukiwania termin który użytkownik udostępnia.
> - Jak ma pola na stronie "Pamiętaj" użytkownik wprowadził.
>   
> 
> Funkcje/technologie omówione:
> 
> - `Request` Obiektu.
> - SQL `Where` klauzuli.


## <a name="what-youll-build"></a>Jakie będzie kompilacji

Poprzednie samouczka utworzono bazy danych, do niego dodana danych, a następnie używane `WebGrid` pomocnika do wyświetlania danych. W tym samouczku zostanie dodana pole wyszukiwania, która umożliwia wyszukać filmy określonego rodzaju lub tytuł której zawiera niezależnie od programu word, możesz wprowadzić. (Na przykład można znaleźć wszystkie filmy, którego genre jest "Akcja" lub których tytuł zawiera "Harry" lub "Adventure.")

Po zakończeniu korzystania z tego samouczka, masz strony podobne do następującego:

![Strona filmy o rodzaju i tytuł wyszukiwania](form-basics/_static/image1.png)

Listę część strony jest taki sam jak ostatniego samouczek &mdash; siatki. Różnica będzie czy siatki będą wyświetlane tylko filmy Przeszukano dla.

## <a name="about-html-forms"></a>Formularze HTML — informacje

(Jeśli masz doświadczenie w tworzeniu formularzy HTML i z tą różnicą między `GET` i `POST`, możesz pominąć tę sekcję.)

Formularz zawiera elementów wejściowych użytkownika &mdash; pola tekstowe, przyciski, przyciski radiowe, pola wyboru, listy rozwijane i tak dalej. Użytkownicy wypełnienie tych kontrolek lub opcje, a następnie prześlij go, klikając przycisk.

W tym przykładzie przedstawiono podstawowe składni języka HTML w postaci:

[!code-html[Main](form-basics/samples/sample1.html)]

Po uruchomieniu tego znaczników na stronie tworzy prosty formularz, który wygląda jak na ilustracji:

![Podstawowa formularza HTML jako renderowany w przeglądarce](form-basics/_static/image2.png)

`<form>` Element umieszcza elementów HTML do przesłania. (Jest łatwe błędu, aby dodać elementy do strony, ale pamiętaj umieścić je w `<form>` elementu. W takim przypadku nie jest przesyłany.) `method` Atrybut informuje przeglądarkę, jak można przesłać dane wejściowe użytkownika. Ustaw tę opcję na `post` przeprowadzania aktualizacji na serwerze lub do `get` Jeśli jest tylko pobieranie danych z serwera.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST i bezpieczeństwa zlecenie HTTP**
> 
> HTTP, protokół, który przeglądarek i serwery używają do wymiany informacji, jest niezwykle proste w jego podstawowych operacji. Przeglądarki używają tylko kilka zlecenia na wysyłanie żądań do serwerów. Podczas pisania kodu dla sieci web, warto poznać te zleceń i jak przeglądarką i serwerem z nich korzystać. I daleko zleceń najczęściej używane są te:
> 
> - `GET`. Przeglądarka używa tego żądania można pobrać elementu z serwera. Na przykład po wpisaniu adresu URL w przeglądarce wykonuje przeglądarka `GET` operację żądania strony. Jeśli strona zawiera grafiki, przeglądarka wykonuje dodatkowe `GET` operacje można pobrać obrazów. Jeśli `GET` operacja ma do przekazywania informacji do serwera, informacje są przekazywane jako część adresu URL w ciągu zapytania.
> - `POST`. Przeglądarka wysyła `POST` żądania w celu przesyłania danych do dodania lub zmieniony na serwerze. Na przykład `POST` zlecenia służy do tworzenia rekordów w bazie danych lub zmienić istniejące. W większości przypadków, gdy Wypełnij formularz i kliknij przycisk Zatwierdź, wykonuje przeglądarka `POST` operacji. W `POST` operacji, dane były przekazywane do serwera znajdują się w treści strony.
> 
> Jest to ważna różnica między tymi zleceń `GET` operacji nie ma zmian na serwerze — lub umieść ją w sposób nieco bardziej abstrakcyjny `GET` operacji nie powoduje zmianę stanu na serwerze. Można wykonywać `GET` operacji na tyle samo zasobów co tyle razy, ile Ci się podoba i nie należy zmieniać tych zasobów. (A `GET` operacji jest często nazywany "bezpiecznej", lub użyj termin techniczny, jest *idempotentności*.) Z drugiej strony, oczywiście `POST` żądania zmiany wystąpił na serwerze zawsze można wykonać operacji.
> 
> Dwa przykłady pomogą ilustrują tej różnicy. Podczas wyszukiwania przy użyciu aparatu, takiej jak Bing czy Google, wypełnij formularz, który składa się z jednego pola tekstowego, a następnie kliknij przycisk wyszukiwania. Wykonuje przeglądarka `GET` operacji o wartości wprowadzony w polu przekazanych jako część adresu URL. Przy użyciu `GET` operacji dla tego typu formularza jest poprawnie, ponieważ operacja wyszukiwania nie zmienia żadnych zasobów na serwerze, po prostu pobiera informacje.
> 
> Teraz Rozważmy proces porządkowania coś w trybie online. Należy wypełnić szczegółów zamówienia, a następnie kliknij przycisk przesyłania. Ta operacja zostanie `POST` żądania, ponieważ operacja spowoduje zmiany na serwerze, na przykład nowego rekordu zlecenia, zmiany w danych konta i prawdopodobnie wiele innych zmian. W odróżnieniu od `GET` operacji, nie można powtórzyć Twojej `POST` żądania — Jeśli Ty, zawsze możesz ponownie wysłane żądanie, czy wygenerować nową kolejność na serwerze. (W takich sytuacjach witryn sieci Web będzie często ostrzegać do nie więcej niż raz kliknij przycisk Zatwierdź lub spowoduje wyłączenie przycisku Prześlij, dzięki czemu użytkownik nie ponownie Prześlij formularz przypadkowo.)
> 
> W ramach tego samouczka użyjesz zarówno `GET` operacji i `POST` operacji do pracy z formularzy HTML. Wyjaśniamy w każdej sprawy Dlaczego zlecenie, którego używasz, jest odpowiedni.
> 
> (Aby dowiedzieć się więcej na temat zleceń HTTP, zobacz [definicjami metod](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artykuł w witrynie W3C.)


Większość elementów wejściowych użytkownika są HTML `<input>` elementów. Wyglądają `<input type="type" name="name">,` gdzie *typu* wskazuje typ kontrolki wejściowe użytkownika mają. Najczęściej spotykane są to następujące elementy:

- Pole tekstowe: `<input type="text">`
- Pole wyboru: `<input type="check">`
- Przycisk radiowy: `<input type="radio">`
- przycisk: `<input type="button">`
- Przycisk Prześlij: `<input type="submit">`

Można również użyć `<textarea>` element, aby utworzyć wielowierszowego pola tekstowego i `<select>` elementu z listy rozwijanej lub przewijanej listy. (Aby uzyskać więcej informacji na temat HTML tworzą elementów, zobacz [formularzy HTML i dane wejściowe](http://www.w3schools.com/html/html_forms.asp) w witrynie W3Schools.)

`name` Atrybut jest bardzo ważne, ponieważ nazwa jest sposób otrzymasz wartość elementu później, jak można zauważyć wkrótce.

Część interesujące jest Tobie, deweloperze strony, czego danych wejściowych użytkownika. Brak zachowania wbudowanych powiązane z tymi elementami. Zamiast tego należy uzyskać wartości, które użytkownik ma wprowadzić lub wybrane i Zrób coś z nimi. To jest zawartość w tym samouczku.

> [!TIP] 
> 
> **HTML5 i zwykłe formularze**
> 
> Może wiesz, HTML jest w stanie przejścia i najnowszej wersji (HTML5) obsługuje bardziej intuicyjne dla użytkowników do wprowadzania informacji. Na przykład w języku HTML5, (developer strony) można ustalić strony czy chcesz, aby użytkownik musi wprowadzić datę. Przeglądarka następnie może automatycznie wyświetlić kalendarz niż użytkownik musi ręcznie wprowadź datę. Jednak HTML5 nowego i nie jest we wszystkich przeglądarkach jeszcze obsługiwane.
> 
> Strony sieci Web ASP.NET obsługuje HTML5 danych wejściowych w zakresie, w jakim jest przeglądarki użytkownika. Dla wyobrażenie o nowe atrybuty do `<input>` elementu w języku HTML5, zobacz [HTML &lt;wejściowych&gt; atrybutu typu](http://www.w3schools.com/html/html_form_input_types.asp) w witrynie W3Schools.


## <a name="creating-the-form"></a>Tworzenie formularza

W programie WebMatrix w **pliki** obszaru roboczego, otwórz *Movies.cshtml* strony.

Po upływie `</h1>` tagu i przed rozpoczęciem `<div>` tagu `grid.GetHtml` wywołanie, Dodaj następujący kod:

[!code-html[Main](form-basics/samples/sample2.html)]

Ten kod znaczników tworzy formularz zawierający pole tekstowe o nazwie `searchGenre` i przycisk przesyłania. Tekst pola i przesłać przycisk są ujęte w `<form>` element których `method` atrybut ma ustawioną `get`. (Należy pamiętać, że jeśli nie zostanie umieszczony pola tekstowego, przycisk wewnątrz Prześlij `<form>` elementu, nic nie zostanie przesłany, po kliknięciu przycisku.) Możesz użyć `GET` zlecenie tutaj ponieważ tworzysz formularza który nie wprowadzać żadnych zmian na serwerze — wystarczy powoduje wyszukiwanie. (W poprzednim samouczku użyto `post` metodę, która jest jak przesłać zmian na serwer. Użytkownik będzie wyświetlana w następnym samouczku ponownie.)

Uruchom strony. Mimo że jeszcze nie zdefiniowano żadnych zachowanie formularza, można wyświetlić wygląda następująco:

![Strona filmów z pola wyszukiwania dla rodzaju](form-basics/_static/image3.png)

Wprowadź wartość w polu tekstowym, takich jak "Komedia." Następnie kliknij przycisk **Genre wyszukiwania**.

Zanotuj adres URL strony. Ponieważ ustawić `<form>` elementu `method` atrybutu `get`, wprowadzona wartość jest obecnie częścią ciągu zapytania w adresie URL w następujący sposób:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Odczytywanie wartości formularza

Strona zawiera już niektóre kodu, który pobiera dane z bazy danych i wyświetla wyniki w siatce. Teraz należy dodać kod odczytuje wartość pola tekstowego, dzięki czemu można uruchomić zapytanie SQL, która obejmuje terminu wyszukiwania.

Ponieważ metoda formularza jest ustawiona na `get`, możesz przeczytać wartość, która została wprowadzona w polu tekstowym przy użyciu kodu podobnie do następującej:

`var searchTerm = Request.QueryString["searchGenre"];`

`Request.QueryString` Obiektu ( `QueryString` właściwość `Request` obiektu) zawiera wartości elementów, które zostały przesłane w ramach `GET` operacji. `Request.QueryString` Właściwość zawiera *kolekcji* (lista) wartości, które są przesyłane w formularzu. Aby pobrać wszystkie poszczególne wartości, należy określić nazwę elementu, który ma. Dlatego musisz mieć `name` atrybutu `<input>` elementu (`searchTerm`) tworzącą pola tekstowego. (Aby uzyskać więcej informacji na temat `Request` obiektów, zobacz [paska bocznego](#BKMK_TheRequestObject) później.)

To proste można odczytać wartości pola tekstowego. Jednak jeśli użytkownik nie wprowadzono żadnej cały w polu tekstowym, ale kliknięty **wyszukiwania** mimo wszystko, możesz zignorować kliknij ten przycisk, ponieważ nie ma nic do wyszukiwania.

Następujący kod jest przykładem pokazuje, jak wdrożyć te warunki. (Nie masz jeszcze Dodaj ten kod, należy to zrobić później.)

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Test dzieli w ten sposób:

- Pobierz wartość `Request.QueryString["searchGenre"]`, czyli wartość, która została wprowadzona w `<input>` elementu o nazwie `searchGenre`.
- Sprawdź, czy jest ona pusta przy użyciu `IsEmpty` metody. Ta metoda jest standardowym sposobem, aby określić, czy element (na przykład element form) zawiera wartość. Ale naprawdę, obsługę tylko, jeśli ma ona *nie* pusta, w związku z tym...
- Dodaj `!` operator przed `IsEmpty` testu. ( `!` Operator oznacza NEGACJA).

W języku angielskim zwykły, całą `if` warunku przekłada się na następujących: *Jeśli element searchGenre formularza nie jest pusta, następnie...*

Ten blok ustawia etap tworzenia kwerendy korzystającej z terminu wyszukiwania. Należy to zrobić w następnej sekcji.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **Obiekt żądania**
> 
> `Request` Obiektu zawiera wszystkie informacje wysyłane przez przeglądarkę do aplikacji po żądanie lub przesłane strony. Ten obiekt zawiera wszystkie informacje, które użytkownik udostępnia, takie jak wartości pole tekstowe lub plik do przekazania. Zawiera również szerokiej gamy dodatkowe informacje, takie jak pliki cookie, wartości w ciągu kwerendy adresu URL (jeśli istnieje), ścieżkę pliku strony, która jest uruchomiona, typ przeglądarki, które jest używane przez użytkownika, listę języków, które są ustawione w przeglądarce i wiele innych funkcji.
> 
> `Request` Obiekt jest *kolekcji* (lista) wartości. Określając jej nazwę można uzyskać poszczególnych wartości z kolekcji:
> 
> `var someValue = Request["name"];`
> 
> `Request` Obiekt faktycznie udostępnia kilka podzestawy. Na przykład:
> 
> - `Request.Form` podaje wartości od elementów wewnątrz przesłanych `<form>` elementu, jeśli żądanie jest `POST` żądania.
> - `Request.QueryString` podano tylko wartości w ciągu zapytania w adresie URL. (W adresie URL, takie jak `http://mysite/myapp/page?searchGenre=action&page=2`, `?searchGenre=action&page=2` sekcji adresu URL jest ciąg zapytania.)
> - `Request.Cookies` Kolekcja umożliwia dostęp do plików cookie wysłanych przez przeglądarkę.
> 
> Aby pobrać wartości, który znajduje się w przesłanego formularza, możesz użyć `Request["name"]`. Alternatywnie, można zastosować bardziej szczegółowych wersji `Request.Form["name"]` (dla `POST` żądania) lub `Request.QueryString["name"]` (dla `GET` żądania). Oczywiście *nazwa* jest nazwą elementu do pobrania.
> 
> Nazwa elementu, który chcesz pobrać musi być unikatowe w kolekcji, którego używasz. Dlatego `Request` obiektu zawiera pliki, takich jak `Request.Form` i `Request.QueryString`. Załóżmy, że strona zawiera element formularza o nazwie `userName` i *również* zawiera plik cookie o nazwie `userName`. Jeśli otrzymasz `Request["userName"]`, jest niejednoznaczny, w mają wartość formularza lub plik cookie. Jednak jeśli `Request.Form["userName"]` lub `Request.Cookie["userName"]`, jest on jawne, o których wartości do pobrania.
> 
> Dobrym rozwiązaniem określonych oraz używanie podzbiór jest `Request` że jesteś zainteresowany, takich jak `Request.Form` lub `Request.QueryString`. W przypadku prostego stron, które tworzysz, w tym samouczku prawdopodobnie nie naprawdę rozsądne różnic. Jednak podczas tworzenia bardziej złożonych stron, za pomocą jawnego wersji `Request.Form` lub `Request.QueryString` pozwala uniknąć problemów, które mogą wystąpić, gdy strona zawiera formularza (lub wielu formularzy), plików cookie, wartości ciągu zapytania i tak dalej.


## <a name="creating-a-query-by-using-a-search-term"></a>Tworzenie zapytania przy użyciu terminu wyszukiwania

Teraz, gdy wiesz, jak uzyskać terminu wyszukiwania, które użytkownik wprowadził, można utworzyć kwerendę, która korzysta z niego. Należy pamiętać, że można pobrać wszystkich elementów film z bazy danych programu, używasz zapytanie SQL, która wygląda jak tej instrukcji:

`SELECT * FROM Movies`

Można uzyskać tylko niektórych filmów, należy użyć kwerendę, która obejmuje `Where` klauzuli. Klauzulę pozwala ustawić warunek, na którym wierszy zwracanych przez zapytanie. Oto przykład:

`SELECT * FROM Movies WHERE Genre = 'Action'`

Jest podstawowy format `WHERE column = value`. Można używać różnych operatorów, oprócz just `=`, takiej jak `>` (większe niż) `<` (poniżej), `<>` (nie równa się), `<=` (mniejsze lub równe), itd., w zależności od tego, czego szukasz.

W przypadku, gdy jest zastanawiasz się instrukcji SQL nie jest uwzględniana &mdash; `SELECT` jest taka sama jak `Select` (lub nawet `select`). Jednak użytkownicy często wielką słów kluczowych w instrukcji SQL, tak samo, jak `SELECT` i `WHERE`, aby był bardziej czytelny.

### <a name="passing-the-search-term-as-a-parameter"></a>Przekazywanie wyszukiwany termin jako parametr

Wyszukiwanie określonego rodzaju jest dość proste (`WHERE Genre = 'Action'`), ale chcesz mieć możliwość wyszukiwania dla dowolnego rodzaju wprowadzonych przez użytkownika. W tym celu Utwórz jako zapytanie SQL, która zawiera symbol zastępczy wartość do wyszukania. To polecenie będzie wyglądać:

`SELECT * FROM Movies WHERE Genre = @0`

Symbol zastępczy jest `@` znak następuje zero. Jak może przypuszczalnie zapytanie może zawierać wiele symboli zastępczych i będą miały postać `@0`, `@1`, `@2`itp.

Aby skonfigurować zapytania i faktycznie wartość należy przekazać go, należy użyć kodu podobne do poniższych:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Ten kod jest podobny do co zostało już wykonane do wyświetlenia w siatce danych. Tylko różnice są następujące:

- Zapytanie zawiera symbol zastępczy (`WHERE Genre = @0"`).
- Zapytania są umieszczane w zmiennej (`selectCommand`); przed przekazane zapytanie bezpośrednio do `db.Query` metody.
- Podczas wywoływania `db.Query` metody, Przekaż zarówno zapytania i wartość symbolu zastępczego. (Jeśli zapytanie ma wiele symboli zastępczych, czy przekazać je wszystkie jako osobne wartości do metody.)

Jeśli te elementy jednocześnie, otrzymasz następujący kod:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Ważne!** Przy użyciu symboli zastępczych (takich jak `@0`) do przekazania wartości do polecenia SQL jest *bardzo ważne* zabezpieczeń. Sposób możesz w tym miejscu, zobacz z symbole zastępcze danych zmiennej jest jedynym sposobem, należy konstruować poleceń SQL.
> 
> Nigdy nie Skonstruuj instrukcję SQL przez zestawienie (łączenie) literały tekstowe i wartości, które można uzyskać od użytkownika. Łączenie danych wejściowych użytkownika do instrukcji SQL otwiera witryny *ataku polegającego na iniekcji SQL* gdzie złośliwy użytkownik przesyła wartości do strony, które hack bazy danych. (Więcej w artykule [iniekcja kodu SQL](https://msdn.microsoft.com/library/ms161953.aspx) witryny sieci Web MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Aktualizowanie strony filmów z kodem wyszukiwania

Teraz możesz zaktualizować kodu w *Movies.cshtml* pliku. Aby rozpocząć, Zastąp kod w bloku kodu, w górnej części strony ten kod:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

Różnica polega na tym została zapytania do `selectCommand` zmiennej, która będzie przekazać do `db.Query` później. Umieszczanie w zmiennej instrukcji SQL pozwala zmienić instrukcję, która jest, co możesz zrobić do przeszukiwania.

Po usunięciu także te dwa wiersze, które można będzie odłożyć później:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Nie chcesz uruchomić kwerendę jeszcze (to znaczy wywołać `db.Query`) i nie chcesz zainicjować `WebGrid` pomocnika jeszcze albo. Należy to zrobić tych problemów po możesz już znalezienia instrukcję SQL, która musi zostać uruchomione.

Po tym ponownie zapisane bloku można dodać nowego logikę Obsługa wyszukiwania. Kompletny kod będzie wyglądać następująco. Zaktualizuj kod na stronie, dopasowując go w tym przykładzie:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

Jak to działa teraz strony. Za każdym razem, gdy strona działa kod otwiera bazę danych i `selectCommand` zmienna jest ustawiona na instrukcję SQL, która pobiera wszystkie rekordy z `Movies` tabeli. Kod również inicjuje `searchTerm` zmiennej.

Jednak jeśli bieżącego żądania zawiera wartość `searchGenre` element, ustawia kod `selectCommand` do innego zapytania — mianowicie na taki, który zawiera `Where` klauzuli do wyszukiwania określonego rodzaju. Ustawia również `searchTerm` do niezależnie od została przekazana do pola wyszukiwania (co może mieć wartości nothing).

Niezależnie od języka SQL Instrukcja znajduje się w `selectCommand`, następnie wywołuje kod `db.Query` uruchomić zapytanie, przekazanie jej niezależnie od ma `searchTerm`. Jeśli nie ma `searchTerm`, nie ma znaczenia, ponieważ nie istnieje w tym przypadku nie parametr do przekazania wartości do `selectCommand` mimo to.

Na koniec kod inicjuje `WebGrid` pomocnika za pomocą wyniki zapytania, tak jak wcześniej.

Można stwierdzić, że umieszczenie instrukcji SQL i wyszukiwany termin do zmiennych, dodano elastyczność w kodzie. Jak można zauważyć w dalszej części tego samouczka, można użyć tego podstawową strukturę i dodawaj logiki dla różnych typów wyszukiwania.

## <a name="testing-the-search-by-genre-feature"></a>Testowanie funkcji wyszukiwania według rodzaju

W programie WebMatrix, uruchom *Movies.cshtml* strony. Zostanie wyświetlona strona z pola tekstowego dla rodzaju.

Wprowadź genre, możesz wprowadzono dla jednego z rekordami testu, a następnie kliknij przycisk **wyszukiwania**. Teraz wyświetlić listę tylko filmy zgodne tego rodzaju:

![Filmy stronę po zakończeniu wyszukiwania dla rodzaju "Comedies"](form-basics/_static/image4.png)

Wprowadź innego rodzaju, a następnie ponowić wyszukiwanie. Spróbuj wprowadzić genre przy użyciu wszystkie małe lub wielkie litery, tak aby widać, że wyszukiwanie nie jest uwzględniana wielkość liter.

## <a name="remembering-what-the-user-entered"></a>"Zapamiętywanie" użytkownik wprowadził

Zwróć uwagę, że po wprowadzone określonego rodzaju i kliknięciu **Genre wyszukiwania**, był wyświetlany na liście w tym rodzaju. Jednak polu tekstowym wyszukiwania był pusty &mdash; innymi słowy, nie Pamiętaj, co użytkownik wprowadził.

Należy zrozumieć, dlaczego występuje ten problem. Po przesłaniu strony przeglądarki wysyła żądanie do serwera sieci web. Gdy ASP.NET pobiera żądanie, tworzy wystąpienie całkowicie nowy strony, uruchamia kod w nim i następnie ponownie renderuje stronę w przeglądarce. W efekcie, strony nie może ustalić, czy po zakończeniu pracy w poprzedniej wersji samej siebie. Wszystkie wie, jest jego Otrzymano żądanie, który miał niektóre dane formularza w nim.

Za każdym razem, gdy żądania strony &mdash; po raz pierwszy lub poprzez przesłanie go &mdash; przygotowujemy nowej strony. Serwer sieci web ma Brak pamięci ostatniego żądania. Ani nie ASP.NET, a nie jest przeglądarki. Jedyne połączenie między te osobnych wystąpień strony jest wszystkie dane przesyłane między nimi. Po przesłaniu strony, na przykład nowe wystąpienie strony można uzyskać wysłaną przez wystąpienie starszych danych formularza. (W inny sposób przekazywania danych między stronami jest używanie plików cookie).

Posiadanie sposób opisywania tej sytuacji jest oznacza, że strony sieci web są *bezstanowych*. Serwery sieci Web i strony i elementy na stronie nie obsługują żadnych informacji o stanie poprzedniej strony. Witryna sieci web została zaprojektowana w ten sposób, ponieważ zachowanie stanu poszczególnych żądań może szybko spalin zasoby serwerów sieci web, w których często obsługiwania tysięcy, być może nawet setki tysięcy żądań na sekundę.

Tak właśnie pola tekstowego jest pusta. Po przesłaniu strony ASP.NET utworzone nowe wystąpienie klasy strony i został uruchomiony za pośrednictwem kodu i znaczników. Nie było nic, w tym kod, który informację, ASP.NET, aby umieścić wartość w polu tekstowym. Dlatego ASP.NET nie wykonywać żadnych czynności, a pole tekstowe zostało uznane bez wartości w jego.

Brak faktycznie prosty sposób, aby uniknąć problemów tego problemu. Genre, wprowadzona w polu tekstowym *jest* dostępne w kodzie &mdash; w `Request.QueryString["searchGenre"]`.

Zaktualizuj kod znaczników dla pola tekstowego, aby `value` wartość pochodzi atrybutu `searchTerm`, w tym przykładzie, takich jak:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Na tej stronie można również ustawiono `value` atrybutu `searchTerm` zmiennej, ponieważ w tej zmiennej zawiera także genre wprowadzona. Jednak przy użyciu `Request` obiekt, aby ustawić `value` atrybutu, jak pokazano w tym miejscu jest standardowym sposobem wykonania tego zadania. (Zakładając, że chcesz to zrobić nawet &mdash; w niektórych sytuacjach możesz chcieć renderowania strony *bez* wartości w polach. Zależy to od co się dzieje z aplikacją.)

> [!NOTE]
> Nie można "zapamiętać" wartość pola tekstowego używanego hasła. Byłoby luka w zabezpieczeniach aby umożliwić użytkownikom wypełnić pole hasła przy użyciu kodu.


Ponownie uruchom strony, wprowadź określonego rodzaju, a następnie kliknij przycisk **Genre wyszukiwania**. Teraz nie tylko możesz wyświetlić wyniki wyszukiwania, ale pole tekstowe pamięta, co wprowadzony czas ostatniego:

![Strona wyświetlająca, czy pole tekstowe ma "zapamiętanych" poprzedniego zapisu](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Wyszukiwanie dowolny wyraz w tytule

Teraz możesz wyszukać dowolnego rodzaju, ale można także wyszukać tytuł. Trudno jest uzyskać tytuł PRAWDA podczas wyszukiwania, dlatego zamiast tego możesz wyszukać słowa, które pojawia się w dowolnym miejscu wewnątrz tytuł. Aby to zrobić w programie SQL, należy użyć `LIKE` operatora i składni, takie jak następujące:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

To polecenie pobiera wszystkie filmy o tytułach zawierają "adventure". Jeśli używasz `LIKE` operatora, obejmują wieloznacznego `%` w ramach terminu wyszukiwania. Wyszukiwanie `LIKE 'adventure%'` oznacza "począwszy od"adventure"". (Technicznie, oznacza to "string"adventure", po której następuje niczego.") Podobnie, wyszukiwany termin `LIKE '%adventure'` oznacza "wszystko następuje ciąg"adventure"", w sposób znaczy "zakończone"adventure"".

Wyszukiwany termin `LIKE '%adventure%'` w związku z tym oznacza, że "z"adventure"dowolne miejsce w tytule." (Z technicznego punktu widzenia "wszystko w tytule, następuje"adventure", następuje niczego.")

Wewnątrz `<form>` elementu, Dodaj następujący kod bezpośrednio w zamknięcia `</div>` tag wyszukiwania genre (tuż przed zamknięciem `</form>` element):

[!code-html[Main](form-basics/samples/sample10.html)]

Kod obsługujący to wyszukiwanie jest podobny do kodu wyszukiwania genre, z tą różnicą, że masz można złożyć `LIKE` wyszukiwania. Wewnątrz bloku kodu, w górnej części strony, Dodaj `if` zablokować tuż po `if` blok dla rodzaju wyszukiwania:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Ten kod używa tej samej logiki był wyświetlany poprzednio, z wyjątkiem tego, że wyszukiwanie używa `LIKE` operatora i naraża kod "`%`" przed i po terminu wyszukiwania.

Zwróć uwagę, jak było łatwo dodać inne wyszukiwania do strony. To wszystko, co trzeba było zrobić:

- Utwórz `if` bloku, które przetestowane, aby zobaczyć, czy pole wyszukiwania odpowiednich miał wartość.
- Ustaw `selectCommand` zmienną instrukcję SQL.
- Ustaw `searchTerm` zmiennej na wartość do przekazania do zapytania.

Oto bloku kompletny kod zawiera logikę nowego wyszukiwania tytułu:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Poniżej przedstawiono podsumowanie czego ten kod:

- Zmienne `searchTerm` i `selectCommand` są inicjowane u góry. Zamierzasz ustawioną tych zmiennych odpowiednie wyszukiwany (jeśli istnieje) i odpowiednie polecenie SQL oparte na użytkownik wykona na stronie. Domyślnie jest to prosty pobierania wszystkich filmów z bazy danych.
- W testach dla `searchGenre` i `searchTitle`, ustawia kod `searchTerm` wartość chcesz wyszukać. Te bloki kodu również ustawić `selectCommand` na odpowiednie polecenie SQL dla tego wyszukiwania.
- `db.Query` Metoda jest wywoływana tylko raz przy użyciu dowolnego polecenia SQL znajduje się w `selectedCommand` i niezależnie od wartości znajduje się w `searchTerm`. Jeśli brak szukanego terminu (nie genre i word nie tytułu), wartość `searchTerm` jest pustym ciągiem. Jednakże, który nie ma znaczenia, ponieważ w takim przypadku zapytania nie wymaga parametru.

## <a name="testing-the-title-search-feature"></a>Testowanie funkcji wyszukiwania tytułu

Teraz możesz przetestować strony wyszukiwania ukończone. Uruchom *Movies.cshtml*.

Wprowadź określonego rodzaju, a następnie kliknij przycisk **Genre wyszukiwania**. Siatka wyświetla filmy tego rodzaju, takich jak przed.

Wprowadź słowa tytułu, a następnie kliknij przycisk **wyszukiwania tytułu**. Siatka wyświetla filmy ten wyraz w tytule.

![Po zakończeniu wyszukiwania dla "" w tytule stronę filmy](form-basics/_static/image6.png)

Pozostaw puste obu polach tekstowych, a następnie kliknij przycisk albo. Siatka wyświetla wszystkie filmy.

## <a name="combining-the-queries"></a>Łączenie zapytań

Można zauważyć wyłącznego czy wyszukiwania, które można wykonywać. Tytuł i genre nie może wyszukać w tym samym czasie, nawet jeśli oba pola wyszukiwania wartości w nich. Na przykład nie można przeszukiwać wszystkie filmy akcji, których tytuł zawiera "Adventure". (Jak strony zakodowanej teraz, po wprowadzeniu wartości dla rodzaju i tytułu, przeszukaj tytuły pobiera pierwszeństwo). Aby utworzyć łączącą warunki wyszukiwania, czy trzeba Utwórz zapytanie SQL, które ma składnię podobne do następujących:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

I będzie musiał uruchomić zapytanie przy użyciu instrukcji w następujący (około mówiąc):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Tworzenie logiki, aby umożliwić permutacji wiele kryteriów wyszukiwania można angażowanie się nieco, jak widać. W związku z tym będzie zatrzymać w tym miejscu.

## <a name="coming-up-next"></a>Powtarzający się dalej

W następnym samouczku utworzysz stronę, która używa formularza, aby umożliwić użytkownikom dodawanie filmów w bazie danych.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Pełna lista strony Movie (zaktualizowany o wyszukiwania)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Klauzuli SQL WHERE](http://www.w3schools.com/sql/sql_where.asp) w witrynie W3Schools
- [Metoda definicje](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artykuł w witrynie W3C

> [!div class="step-by-step"]
> [Poprzednie](displaying-data.md)
> [dalej](entering-data.md)
