---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Wprowadzenie do składnika ASP.NET Web Pages — podstawy programowania | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'W tym samouczku przedstawiono omówienie jak do programu ASP.NET Web Pages o składni Razor. Dowiesz się: podstawowej składni "Razor", której użyjesz dla żądania ściągnięcia...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: ec1c055d1b3f6ca5c6374a18840c2595bb368e0e
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021537"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Wprowadzenie do wzorca ASP.NET Web Pages — podstawy programowania
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym samouczku przedstawiono omówienie jak do programu ASP.NET Web Pages o składni Razor.
> 
> Zawartość:
> 
> - Podstawowa składnia "Razor", używanej do programowania w produkcie ASP.NET Web Pages.
> - Niektóre podstawowe języka C#, czyli języka programowania, które będą używane.
> - Niektóre podstawowe pojęcia dotyczące programowania dla stron sieci Web.
> - Jak zainstalować pakiety (składniki, które zawierają wstępnie kodu) za pomocą witryny.
> - Jak używać *pomocników* do wykonywania typowych zadań programistycznych.
>   
> 
> Funkcje/technologie omówione:
> 
> - I Menedżer pakietów NuGet.
> - `Gravatar` Pomocnika.


Niniejszy samouczek jest głównie w ramach ćwiczenia w wprowadzenie do programowania składni, którego będziesz używać dla stron ASP.NET Web Pages. Poznasz *składni Razor* i język programowania kodu, który został napisany w języku C#. Masz możliwość wypróbowania tej składni w poprzednim samouczku; w tym samouczku wyjaśnimy więcej składni.

Gwarantujemy, że ten samouczek obejmuje większość programowania zobaczysz w pojedynczej instrukcji i jest jedynym samouczek, który jest *tylko* o programowaniu. W pozostałych samouczków w tym zestawie faktycznie utworzysz stron, które wykonują ciekawe rzeczy.

Zdobędziesz także wiedzę na temat *pomocników*. Obiekt pomocnika jest składnikiem — zapakowane w górę fragment kodu —, można dodać do strony. Pomocnik wykonuje pracę, które w przeciwnym razie może być uciążliwe lub złożone i dlatego należy ręcznie.

## <a name="creating-a-page-to-play-with-razor"></a>Tworzenie strony, aby odtworzyć ze składnią Razor

W tej sekcji będziesz odtwarzania nieco ze składnią Razor, więc otrzymujesz poznać podstawową składnię.

Uruchom program WebMatrix, jeśli nie jest jeszcze uruchomiona. Użyjesz witryny sieci Web utworzonej w poprzednim samouczku ([Rozpoczynanie pracy przy użyciu stron sieci Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Aby otworzyć go ponownie, kliknij przycisk **obszarze Moje witryny** i wybierz polecenie **WebPageMovies**:

![Wyświetlane opcje Otwórz witrynę i obszarze Moje witryny wyróżnione ekranu startowego programu WebMatrix](intro-to-web-pages-programming/_static/image1.png)

Wybierz **pliki** obszaru roboczego.

Na wstążce kliknij **New** Tworzenie strony. Wybierz **CSHTML** i nadaj nowej stronie *TestRazor.cshtml*.

Kliknij przycisk **OK**.

Skopiuj następujące do pliku, zastępując całkowicie, co jest już.

> [!NOTE]
> Podczas kopiowania kodu lub języka znaczników w przykładach w stronę, wcięcia i wyrównanie może nie być taki sam jak w tym samouczku. Wcięcia i wyrównanie nie wpływają na jak kod jest wykonywany, mimo że.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Badanie przykładowa strona

Większość zostanie wyświetlony jest zwykłe HTML. U góry istnieje jednak ten blok kodu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Zwróć uwagę, następujące elementy dotyczące ten blok kodu:

- Znakiem @ ASP.NET informuje, poniżej znajduje się kod Razor, HTML nie. ASP.NET traktują wszystko po znaku jako kod, dopóki nie działa w kod HTML ponownie @. (W tym przypadku to &lt;! DOCTYPE&gt; elementu.
- Nawiasy klamrowe ({i}) należy umieścić blok kodu Razor, jeśli kod ma więcej niż jeden wiersz. Nawiasy klamrowe Poinformuj ASP.NET, gdzie kod dla tego bloku rozpoczyna i kończy.
- / / Znaków mark komentarz — oznacza to, że część kodu, który nie będzie uruchomiony.
- Każda instrukcja musi być zakończona średnikiem (;). (Nie komentarze, mimo że.)
- Można przechowywać wartości w *zmienne*, które są tworzone (*zadeklarować*) przy użyciu var. — słowo kluczowe Kiedy tworzysz zmienną, nadajesz mu nazwę, która może zawierać litery, cyfry i znak podkreślenia (\_). Nazwy zmiennych nie może zaczynać się cyfrą i nie można użyć nazwy programowania — słowo kluczowe (takie jak var).
- Ciągi znaków (na przykład "ASP.NET" i "Stron sieci Web") należy ująć w znaki cudzysłowu. (Muszą być podwójne znaki cudzysłowu). Numery nie są w znaki cudzysłowu.
- Białe znaki spoza znaki cudzysłowu nie ma znaczenia. Podziały wierszy przede wszystkim nie ma znaczenia; wyjątkiem jest, że nie można podzielić ciąg w cudzysłowie w wierszach. Wcięcia i wyrównanie nie ma znaczenia.

To coś, co nie jest jasne, w tym przykładzie, że cały kod jest uwzględniana wielkość liter. Oznacza to, że zmienna TheSum jest zmienną inną niż zmienne, które może być nazwane theSum lub thesum. Podobnie var jest słowem kluczowym, ale nie jest Var.

### <a name="objects-and-properties-and-methods"></a>Obiekty, właściwości i metod

Następnie jest wyrażenie DateTime.Now. W prostych słowach daty/godziny jest *obiektu*. Obiekt to właśnie można programować za pomocą — strony pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta, itp. Obiekty mają co najmniej jeden *właściwości* opisują ich właściwości. Obiekt pola tekstowego jest właściwością Text (między innymi), obiekt żądania ma właściwość adres Url (i inne), wiadomość e-mail ma właściwość od i do właściwości, i tak dalej. Obiektów ma także *metody* , które są "zleceń" mogą wykonywać. Można będzie się praca z obiektami partii.

Jak widać w przykładzie, daty/godziny jest obiekt, który umożliwia program daty i godziny. Ma właściwość o nazwie teraz, która zwraca bieżącą datę i godzinę.

### <a name="using-code-to-render-markup-in-the-page"></a>Przy użyciu kodu do renderowania kodu znaczników na stronie

W treści strony Zwróć uwagę, że:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Ponownie znaku @ mówi ASP.NET tego poniżej jest kod, nie w formacie HTML. Znaczniki można dodać wraz z wyrażeniem kodu i ASP.NET spowoduje, że wartość prawo wyrażenia w tym momencie. W tym przykładzie @a spowoduje, że niezależnie od wartości jest zmienną o nazwie, @product renderuje, niezależnie od rodzaju jest w zmiennej o nazwie produktu i tak dalej.

Nie trzeba się ograniczać do zmiennych, mimo że. W kilku przypadkach, znakiem @ poprzedzający wyrażenie:

- @(\*b) powoduje wyświetlenie iloczyn niezależnie od rodzaju znajduje się w zmiennych i b. ( \* Operator oznacza mnożenia.)
- @(technologia + "" + produkt) powoduje wyświetlenie wartości w zmiennych technologii i produktów po ich złączenie i dodawanie odstęp między. Łączenie ciągów operator (+) jest taka sama jak operator dodawania liczb. ASP.NET zazwyczaj można stwierdzić, czy pracujesz z międzynarodowymi numerami identyfikującymi lub z ciągami i wykonuje właściwe za pomocą operatora +.
- @Request.Url renderuje właściwość Url obiektu żądania. Obiekt żądania zawiera informacje o bieżącym żądaniu za pomocą przeglądarki i oczywiście właściwość adres Url zawiera adres URL tego bieżące żądanie.

Przykład jest też przeznaczona do pokazania, jak pracę na różne sposoby. Można wykonywania obliczeń w bloku kodu, u góry, umieścić wyniki w zmiennej i renderowania zmiennej w znacznikach. Lub możesz zrobić obliczeń w prawo wyrażenia w znaczniku. Zależy od podejścia, którego używasz, co robisz oraz, do pewnego stopnia na własnych preferencji.

### <a name="seeing-the-code-in-action"></a>Wyświetlanie kodu w akcji

Kliknij prawym przyciskiem myszy nazwę pliku, a następnie wybierz **Uruchom w przeglądarce**. Zostanie wyświetlona strona w przeglądarce przy użyciu wartości i wyrażeń rozwiązany na stronie.

![Strona "TestRazor" uruchomiony w przeglądarce](intro-to-web-pages-programming/_static/image2.png)

Spójrz na źródło w przeglądarce.

!['Test Razor' źródła strony w przeglądarce](intro-to-web-pages-programming/_static/image3.png)

Zgodnie z oczekiwaniami z środowiska w poprzednim samouczku jest Brak kodu Razor na stronie. Wszystko, co widzisz są wartości wyświetlane rzeczywistych. Po uruchomieniu strony, możesz sprawiamy, że żądania do serwera sieci web, która jest wbudowana w programie WebMatrix. Po odebraniu żądania ASP.NET rozpoznaje wszystkie wartości i wyrażenia i renderuje ich wartości do strony. Wysyła następnie strony do przeglądarki.

> [!TIP] 
> 
> **Razor i C#**
> 
> Do chwili obecnej mówiliśmy już pracujesz ze składnią Razor. Jest to istotne, ale nie jest pełną wątku. Rzeczywiste języka programowania używasz nosi nazwę *C#*. C# został utworzony przez firmę Microsoft za pośrednictwem lat temu i stało się jedną z podstawowego języków programowania do tworzenia aplikacji Windows. Wszystkie reguły, które w tym samouczku temat nazwać zmienną oraz tworzenie instrukcji i tak dalej są faktycznie wszystkie reguły języka C#.
> 
> Razor w szczególności dotyczy niewielki zestaw konwencje jak osadzić ten kod w stronę. Na przykład Konwencji do oznaczania kodu na stronie, a za pomocą @ {}, aby osadzić blok kodu jest aspektem Razor strony. Pomocnicy również są traktowane jako część Razor. Składnia razor jest używany w większej liczbie miejsc niż po prostu w składniku ASP.NET Web Pages. (Na przykład jest używany w także widoki ASP.NET MVC.)
> 
> Firma Microsoft wspomnieć o to, ponieważ jeśli szukasz informacji na temat programowania stron ASP.NET Web Pages znajdziesz mnóstwo odwołania do aparatu Razor. Jednak wiele te odwołania nie dotyczą co teraz zrobić i dlatego może być mylące. I w rzeczywistości wielu programowania pytania naprawdę mają zostać o pracy w języku C# lub pracy za pomocą platformy ASP.NET. Dlatego spojrzeć specjalnie dla informacji na temat Razor nie może być potrzebne odpowiedzi.


## <a name="adding-some-conditional-logic"></a>Dodawanie niektóre warunkowe logiki

Jest jedną z najważniejszych funkcji o przy użyciu kodu na stronie sieci, można zmienić, co się stanie w zależności od różnych kryteriów. W tej części samouczka możesz będzie Poeksperymentuj z kilka sposobów, aby zmienić informacje wyświetlane na stronie.

Przykład będzie prosty i nieco contrived tak, aby firma Microsoft może skupić się na logikę warunkową. Strona którą utworzysz, będziesz to robić:

- Pokaż inny tekst na stronie, w zależności od tego, czy jest po raz pierwszy ta strona jest wyświetlana, lub czy został kliknięty przycisk do przesyłania strony. Który będzie stanowić pierwszy test warunkowy.
- Wyświetli się komunikat tylko wtedy, gdy wartość jest przekazywana w ciągu zapytania adresu URL (http://...?show=true). Który będzie stanowić drugi test warunkowy.

W programie WebMatrix, Utwórz stronę i nadaj mu nazwę *TestRazorPart2.cshtml*. (Na wstążce kliknij **New**, wybierz **CSHTML**, nadaj plikowi nazwę, a następnie kliknij **OK**.)

Zastąp zawartość tej strony następujących czynności:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Blok kodu, u góry inicjuje zmienną o nazwie wiadomości z tekstem. W treści strony zawartość zmiennej wiadomości są wyświetlane wewnątrz &lt;p&gt; elementu. Zawiera także znaczników &lt;wejściowych&gt; elementu do utworzenia **przesyłania** przycisku.

Uruchom stronę aby zobaczyć, jak działa teraz. Na razie różnią się statyczną stronę nawet wtedy, gdy klikniesz **przesyłania** przycisku.

Wróć do programu WebMatrix. Wewnątrz bloku kodu, Dodaj następujący wyróżniony kod *po* wiersza, która inicjuje komunikat:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Jeśli blok {}

Co właśnie został dodany został if warunku. W kodzie Jeśli warunek ma strukturę następująco:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Warunek do sprawdzenia jest w nawiasach. Musi on mieć wartość lub wyrażenie, które zwraca wartość PRAWDA lub FAŁSZ. Jeśli warunek jest spełniony, program ASP.NET działa instrukcji lub instrukcji, które znajdują się wewnątrz nawiasów klamrowych. (To są *następnie* wchodzi w skład *if, a następnie* logiki.) Jeśli warunek nie jest spełniony, zostanie pominięty bloku kodu.

Poniżej przedstawiono kilka przykładów warunki, które można przetestować w przypadku instrukcji:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Zmienne dla wartości lub względem wyrażenia można przetestować przy użyciu <em>operatora logicznego</em> lub <em>operator porównania</em>: równe (==) większa (&gt;), mniej niż (&lt;), większa niż lub równe (&gt;=) i mniejsze niż lub równe (&lt;=). ! = Oznacza operator nie jest równa — na przykład, jeśli (! = 0) oznacza <em>Jeśli</em> <em>a</em><em>nie jest równa 0</em>.

> [!NOTE]
> Upewnij się, że można zauważyć, że operator porównania dla równości się (==) nie jest taka sama jak wartość =. = — Operator jest używany tylko do przypisywania wartości (var = 2). Jeśli te operatory są mieszały się, albo otrzymasz błąd lub niektóre dziwne wyniki.


Aby sprawdzić, czy coś, co ma wartość true, jest pełną składnię if(IsDone == true). Jednak możesz również użyć if(IsDone) skrótów. Jeśli nie ma żadnego operatora porównania, ASP.NET zakłada, testujesz przypadku wartości true.

! Operator samodzielnie oznacza logiczne NOT. Na przykład, warunek if (! Oznacza, że IsPost) *Jeśli IsPost nie zostanie spełniony*.

Warunki można połączyć za pomocą operator logiczny oraz (&amp; &amp; operator) lub logiczne OR (|| — operator). Na przykład ostatniego, jeśli warunki w poprzednim oznacza przykłady *Jeśli FileProcessingIsDone nie jest ustawiony na wartość true, displayMessage i ma wartość false*.

### <a name="the-else-block"></a>Blok else

Jedno końcowe o tym, czy bloki: Jeśli blok może następować innego bloku. Przydaje się inny blok jest trzeba wykonać różny kod, gdy warunek jest fałszywy. Poniżej przedstawiono prosty przykład:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

W kolejnych samouczkach w tej serii, w którym jest przydatne przy użyciu innego bloku, pojawi się kilka przykładów.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Sprawdzenie, czy żądanie jest przesyłania (post)

Istnieje więcej, ale teraz wrócić do przykładu, który ma if(IsPost) warunek {...}. IsPost jest faktycznie właściwością bieżącej strony. Przy pierwszym żądaniu strony IsPost zwraca wartość false. Jednak jeśli kliknięcie przycisku, lub w przeciwnym razie ją następnie przesłać — oznacza to, że wpis — IsPost zwraca wartość true. Dlatego IsPost pozwala określić, czy jesteś zajmujących przesyłania formularza. (Pod względem zleceń HTTP, jeśli żądanie jest operacją GET IsPost zwraca wartość false. Jeśli żądanie jest operacji POST, IsPost zwraca wartość true.) Później w samouczku będziesz pracować wejściowych formularzy, gdzie ten test staje się szczególnie przydatne.

Uruchom stronę. Ponieważ jest to po raz pierwszy masz żądanej strony, zostanie wyświetlony "Jest po raz pierwszy żądanej strony". Ten ciąg jest wartość, która zainicjować zmienną wiadomości. Jest test if(IsPost), ale zwraca wartość false w chwili, tak aby blokował kod wewnątrz if nie zostanie uruchomiona.

Kliknij przycisk **przesyłania** przycisku. Strona zostanie ponownie wywołana. Jako wcześniej, zmienna wiadomości jest równa "Jest pierwszym...". Jednak tym razem if(IsPost) testu zwraca wartość true, dzięki czemu kod wewnątrz czy zablokować przebiegów. Kod zmienia wartość zmiennej wiadomości na inną wartość, czyli o tym, co jest renderowany w znaczniku.

Teraz Dodaj if warunku w znaczniku. Poniżej &lt;p&gt; element, który zawiera **przesyłania** przycisku, Dodaj następujący kod:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Dodawany kod wewnątrz znaczników, więc trzeba zacząć @. A następnie if jest test podobnej do tej, dodano wcześniej w bloku kodu. Wewnątrz nawiasów klamrowych, jednak w przypadku dodawania zwykłego HTML — co najmniej to zwykłe dopóki nie zostanie osiągnięty do @DateTime.Now. Jest to inny trochę kodu Razor, więc ponownie trzeba dodać przed nim.

Zwrócić uwagę na to, można dodać, jeśli warunki zarówno kod block u góry, a w znaczniku. Jeśli używasz if warunku w treści strony wierszy wewnątrz bloku może być znaczników lub innego kodu. W takiej sytuacji i podobnie jak w dowolnym momencie możesz mieszać znaczników i kodu, należy użyć się to gotowość do ASP.NET gdy kod jest.

Uruchom stronę, a następnie kliknij przycisk **przesyłania**. Teraz nie tylko widzisz inny komunikat podczas przesyłania ("po przesłaniu..."), ale zostanie wyświetlony nowy komunikat, który wyświetla datę i godzinę.

![Prześlij stronie 'Test aparatu Razor 2', uruchomiony w przeglądarce z sygnaturą czasową wyświetlane po](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testowanie wartości ciągu zapytania

Co więcej testów. Ten czas, jaki dodasz if blok, który sprawdza czy wartość o nazwie show, które mogą być przekazywane w ciągu zapytania. (Podobnie do następującego: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) poznasz, jak zmienić stronę, aby wiadomości możesz już został wyświetlania ("Jest pierwszym..." itp.) jest wyświetlana tylko wtedy, jeśli wartość show ma wartość true.

U dołu (ale wewnątrz) bloku kodu, w górnej części strony, Dodaj następujący fragment kodu:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Kompletny kod wygląd teraz blok jak w poniższym przykładzie. (Należy pamiętać, że po skopiowaniu kodu na stronie wcięcie może wyglądać inaczej. Ale który nie ma wpływu na sposób uruchamiania kodu).

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Nowy kod w bloku inicjuje zmienną o nazwie obiektu na wartość false. Następnie wykonuje if testu do wyszukiwania wartości w ciągu zapytania. W przypadku najpierw żądania strony, ma adres URL podobny do poniższego:

`http://localhost:43097/TestRazorPart2.cshtml`

Kod określa, czy adres URL zawiera zmienną o nazwie show w ciągu zapytania, podobnie jak w tej wersji adresu URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Pokaż = true

Badanie sprawdza właściwości QueryString obiektu żądania. Ciąg zapytania zawiera element o nazwie show, a ten element jest ustawiona na wartość true, jeśli blok uruchamia i ustawia zmienną obiektu na wartość true.

Jak widać jest sposobem, w tym miejscu. Jak wynika z nazwy, ciąg zapytania jest ciąg. Jednak możesz tylko przetestować dla wartości true i false Jeśli wartość, którą testujesz jest wartość logiczna (true/false). Przed przetestowaniem wartość zmiennej show w ciągu zapytania, musisz przekonwertować wartość typu Boolean. To jest metoda AsBool — przyjmuje ciąg jako dane wejściowe i konwertuje ją na wartość logiczną. Wyraźnie widać Jeśli ciąg ma wartość "prawda", metoda AsBool konwertuje tę wartość na true. Jeśli wartość ciągu jest inny, AsBool zwraca wartość false.

> [!TIP] 
> 
> **Typy danych i metod As()**
> 
> Firma Microsoft tylko tym do tej pory gdy tworzysz zmienną, użycie var. — słowo kluczowe To nie jest cały artykuł, mimo że. W celu manipulowania wartością — do dodawania numerów, lub ciągów, porównywanie dat lub testowania PRAWDA/FAŁSZ — C# ma do pracy z odpowiedniej wewnętrznej reprezentacji wartości. C# można *zwykle* zorientować się, jakie powinny być tę reprezentację (oznacza to, co *typu* dane) oparte na wykonujesz wartościami. Teraz, a następnie jednak ją tej czynności nie. W przeciwnym razie ma pomagać wskazując jawnie jak C# powinny reprezentować danych. Metoda AsBool robi to — informuje C#, że wartość ciągu "true" lub "false" powinny być traktowane jako wartość logiczna. Istnieją podobne metody do reprezentowania ciągów jako innych typów, takich jak AsInt (traktowanej jako liczba całkowita), AsDateTime (traktowanej jako daty/godziny), AsFloat (traktowanej jako liczba zmiennoprzecinkowa) i tak dalej. Korzystając z tych metod (), jeśli C# nie może reprezentować wartość ciągu zgodnie z żądaniem, zobaczysz błąd.


W znaczniku strony, należy usunąć lub skomentować tego elementu (w tym miejscu wyświetleniem skomentowanej się):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Po prawej stronie, gdzie została usunięta lub oznaczone jako komentarz ten tekst, Dodaj następujący kod:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Jeśli test jest w stanie, jeśli zmienna obiektu ma wartość true, renderowanie &lt;p&gt; element z wartością zmiennej wiadomości.

### <a name="summary-of-your-conditional-logic"></a>Podsumowanie warunkowe logiki

W przypadku, gdy nie masz całkowitej pewności co do co właśnie wykonano, w tym miejscu znajduje się podsumowanie.

- Zmienna wiadomości jest inicjowana na domyślny ciąg ("to jest pierwszym...").
- Jeśli żądanie strony jest wynikiem przesyłania (post), wartość komunikatu jest zmieniona na "po przesłaniu..."
- Zmienna obiektu jest ustawiana na wartość false.
- Jeśli ciąg zapytania zawiera? Pokaż = true, zmienna obiektu jest ustawiona na wartość true.
- W znaczniku, jeśli obiektu ma wartość true &lt;p&gt; element jest renderowany pokazujący wartość komunikatu. (Jeśli obiektu nie jest spełniony, nic nie jest renderowany w tym momencie w znaczniku.)
- W znaczniku, jeśli żądanie jest żądaniem post &lt;p&gt; element jest renderowany, który wyświetla datę i godzinę.

Uruchom stronę. Nie ma, ponieważ obiektu ma wartość false, dzięki czemu w znaczniku testu if(showMessage) zwraca wartość false.

Kliknij przycisk **przesłać**. Zostanie wyświetlony, daty i godziny, ale nadal żaden komunikat.

W przeglądarce przejdź do pola Adres URL i Dodaj następujący element do końca adresu URL:? Pokaż = true, a następnie naciśnij klawisz Enter.

!['Test aparatu Razor 2' strony w przeglądarka wyświetlająca ciąg zapytania](intro-to-web-pages-programming/_static/image5.png)

Ta strona jest wyświetlana ponownie. (Ponieważ zmieniono adres URL jest nowe żądanie nie Prześlij). Kliknij przycisk **przesyłania** ponownie. Komunikat pojawi się ponownie, kiedy jest data i godzina.

![Strona "Test aparatu Razor 2", po przesyłanie po ciągu zapytania](intro-to-web-pages-programming/_static/image6.png)

W adresie URL, należy zmienić? Pokaż = true na? Pokaż = false, a następnie naciśnij klawisz Enter. Ponownie prześlij stronie. Strona jest powrót do sposobu uruchamiania — żaden komunikat.

Jak wspomniano wcześniej, logikę w tym przykładzie jest trochę contrived. Jednakże, jeśli zamierza pojawiają się w wielu stron sieci i potrwa co najmniej jedną z formularzy, w tym samouczku w tym miejscu.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalowanie pomocnika (wyświetlanie obrazu Gravatar)

Niektóre zadania, ludzie często chcemy na stronach sieci web, które wymagają dużej ilości kodu lub dodatkowe wiedzy. Przykłady: wyświetlanie wykresu dla danych. umieszczanie Facebook "Jak" przycisk na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie i zmienianie rozmiaru obrazów; Korzystając z systemu PayPal, dla danej witryny. Aby ułatwić wykonaj te rodzaje elementów, ASP.NET Web Pages umożliwia korzystanie z *pomocników*. Pomocnicy są składnikami instalowanej lokacji i umożliwiające wykonywanie typowych zadań przy użyciu zaledwie kilku wierszy kodu Razor.

Strony ASP.NET Web Pages ma kilka pomocników wbudowane. Wiele wątków, są jednak dostępne w pakietach (dodatki), które są dostarczane przy użyciu Menedżera pakietów NuGet. NuGet pozwala wybrać pakiet do zainstalowania, a następnie ta odpowiada za wszystkie szczegóły instalacji.

W tej części samouczka zainstalujesz pomocnika, który umożliwia wyświetlanie obrazu Gravatar ("Awatar rozpoznany globalnie"). Dowiesz się dwie rzeczy. Jeden to jak Znajdowanie i instalowanie pomocnika. Dowiesz się, jak obiekt pomocnika można łatwo zrobić coś, które w przeciwnym razie trzeba zrobić przy użyciu dużej ilości kodu, trzeba napisać samodzielnie.

Możesz zarejestrować własne Gravatar w witrynie internetowej Gravatar na [ http://www.gravatar.com/ ](http://www.gravatar.com/), ale nie jest istotne, aby utworzyć konto Gravatar do wykonania tej części samouczka.

W programie WebMatrix, kliknij przycisk **NuGet** przycisku.

![Okno dialogowe galerii pakietów NuGet w programie WebMatrix](intro-to-web-pages-programming/_static/image7.png)

To spowoduje uruchomienie Menedżera pakietów NuGet i wyświetla dostępnych pakietów. (Nie wszystkie pakiety są pomocnicy; niektóre dodają funkcje do programu WebMatrix sam niektóre są dodatkowe szablony itd.) Może zostać komunikat o błędzie dotyczący niezgodności wersji. Możesz zignorować ten komunikat o błędzie, klikając **OK** , a następnie za pomocą tego samouczka.

![Okno dialogowe galerii pakietów NuGet w programie WebMatrix](intro-to-web-pages-programming/_static/image8.png)

W polu wyszukiwania wprowadź "pomocników platformy asp.net". NuGet zawiera pakiety, które pasują do kryteriów wyszukiwania.

![Galeria NuGet w programie WebMatrix przedstawiający pakietów](intro-to-web-pages-programming/_static/image9.png)

Bibliotekę pomocników platformy ASP.NET sieci Web zawiera kod, aby uprościć wiele typowych zadań, łącznie z użyciem Gravatar obrazów. Wybierz **bibliotekę pomocników sieci Web platformy ASP.NET** pakietu, a następnie kliknij przycisk **zainstalować** uruchomienia Instalatora. Wybierz **tak** po wyświetleniu monitu, jeśli chcesz zainstalować pakiet, a następnie zaakceptuj warunki, aby ukończyć instalację.

To wszystko. NuGet pobiera i instaluje wszystkim, w tym wszelkie dodatkowe składniki, które mogą być wymagane (*zależności*).

Jeśli zaistnieje należy odinstalować pomocnika, proces jest bardzo podobne. Kliknij przycisk **NuGet** przycisku, kliknij przycisk **zainstalowane** kartę, a następnie wybierz pakiet, który chcesz odinstalować.

## <a name="using-a-helper-in-a-page"></a>Używanie Pomocnika na stronie

Teraz użyjesz pomocnika, który został zainstalowany. Proces dodawania pomocnika do strony jest podobny dla większości pomocników.

W programie WebMatrix, Utwórz stronę i nadaj mu nazwę *GravatarTest.cshml*. (Tworzysz specjalną stroną, aby przetestować pomocnika, ale pomocników można użyć w dowolnej stronie w witrynie).

Wewnątrz &lt;treści&gt; elementu Dodawanie &lt;div&gt; elementu. Wewnątrz &lt;div&gt; elementu, wpisz:

@Gravatar.

Znak @, jest takiego samego znaku wcześniej użyto do oznaczania kodu Razor. **Gravatar** obiekt pomocnika, w którym pracujesz.

Jak najszybciej po wpisaniu kropki (.), program WebMatrix Wyświetla listę *metody* (funkcje) udostępnia pomocnika Gravatar:

![Gravatar pomocnika technologii IntelliSense, listy rozwijanej listy](intro-to-web-pages-programming/_static/image10.png)

Ta funkcja jest znany jako *IntelliSense*. Pomaga ono kodowania, podając odpowiednie w kontekście opcji. Technologia IntelliSense działa z HTML, CSS, kod ASP.NET, JavaScript i innych językach, które są obsługiwane w programie WebMatrix. Jest innym funkcja, która ułatwia tworzenie stron sieci web w programie WebMatrix.

G naciśnij klawisz na klawiaturze, zobacz, że funkcja IntelliSense znajduje metodę GetHtml. Naciśnij klawisz Tab. Funkcja IntelliSense wstawia wybranej metody (GetHtml). Wpisz nawias otwierający i zwróć uwagę, że nawias zamykający jest automatycznie dodawany. Wpisz swój adres e-mail w znaki cudzysłowu między dwoma nawiasami. Jeśli masz konto Gravatar, zostanie zwrócony swoje zdjęcie profilowe. Jeśli nie masz konta Gravatar, zwracany jest domyślnego obrazu. Gdy wszystko będzie gotowe, wiersz wygląda następująco:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Teraz można wyświetlić strony w przeglądarce. Obraz lub domyślny obraz jest wyświetlany w zależności od tego, czy masz konto Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![domyślny obraz](intro-to-web-pages-programming/_static/image12.png)

Aby poznać co robi pomocnika dla Ciebie, Wyświetl źródło strony w przeglądarce. Wraz z HTML, które były dostępne na stronie zostanie wyświetlony element obrazu, który zawiera identyfikator. Jest to kod, który pomocnika renderowany na stronie w miejscu, w którym masz @Gravatar.GetHtml. Pomocnik trwało informacje udostępniane i wygenerowany kod, który komunikuje się bezpośrednio z Gravatar w celu uzyskania ponownie prawidłowy obraz podane konto.

Metoda GetHtml umożliwia również dostosować obraz, zapewniając innych parametrów. Poniższy kod pokazuje, jak żądania obrazu o szerokości i wysokości 40 pikseli i używa obrazu określoną wartość domyślną, o nazwie **wavatar** Jeśli określone konto nie istnieje.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Ten kod tworzy podobny następujące wyniki (domyślny obraz losowo różnią się).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Pojawi się dalej

Aby zachować ten krótki samouczek, musimy skupić się na tylko kilka podstawowych. Oczywiście istnieje *partii* więcej Razor i C#. Dowiesz się więcej, jak przejść przez te samouczki. Jeśli chcesz się dowiedzieć więcej na temat programowania aspektów Razor i C#, teraz możesz przeczytać tutaj bardziej szczegółowe wprowadzenie: [wprowadzenie do platformy ASP.NET sieci Web programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

Następny samouczek wprowadza do pracy z bazą danych. Ten samouczek będzie stosowany, tworzenie przykładowej aplikacji, która pozwala na liście ulubionych filmów.

## <a name="complete-listing-for-testrazor-page"></a>Kompletna lista TestRazor strony

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Kompletna lista TestRazorPart2 strony

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Kompletna lista GravatarTest strony

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Pomocnik usługi Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Poprzednie](getting-started.md)
> [dalej](displaying-data.md)
