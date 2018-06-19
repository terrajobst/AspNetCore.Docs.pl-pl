---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Wprowadzenie do składnika ASP.NET Web Pages — podstawy programowania | Dokumentacja firmy Microsoft
author: tfitzmac
description: 'W tym samouczku przedstawiono omówienie jak do programu ASP.NET Web Pages o składni Razor. Dowiesz się: podstawowa składnia "Razor" używanego programu pr...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 60115dd06a27bf856427953de29e993194afb991
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33839289"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Wprowadzenie do strony sieci Web ASP.NET — podstawy programowania
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym samouczku przedstawiono omówienie jak do programu ASP.NET Web Pages o składni Razor.
> 
> Zawartość:
> 
> - Podstawowa składnia "Razor" używanej do programowania w języku ASP.NET Web Pages.
> - Niektóre podstawowe języka C#, czyli język programowania, które będą używane.
> - Niektóre podstawowe pojęcia dotyczące programowania dla stron sieci Web.
> - Jak zainstalować pakiety (składniki, które zawierają wbudowane kodu) do użycia z witryną.
> - Jak używać *pomocników* do wykonywania typowych zadań programowania.
>   
> 
> Funkcje/technologie omówione:
> 
> - Oraz Menedżer pakietów NuGet.
> - `Gravatar` Pomocnika.


Ten samouczek jest przeznaczony głównie wykonywania w wprowadzenie do programowania składni, które będzie używane dla stron sieci Web programu ASP.NET. Dowiesz się, *składni Razor* i kod napisany w języku C# — język programowania. Otrzymano glimpse tej składni w samouczku poprzedniej; w tej instrukcji wyjaśniamy więcej składni.

Obiecujemy, że ten samouczek obejmuje większość programowania zobaczysz jednego samouczka i jest tylko samouczek, który jest *tylko* o programowania. W pozostałych samouczków w tym zestawie utworzysz faktycznie stron, które są interesujące elementy.

Zawiera również informacje o *pomocników*. Pomocnik jest składnikiem — opakowane w górę fragment kodu — które można dodać do strony. Pomocnik wykonuje pracę za użytkownika, które w przeciwnym razie mogą być niewygodny lub złożonych zrobić ręcznie.

## <a name="creating-a-page-to-play-with-razor"></a>Tworzenie strony, aby odtworzyć ze składnią Razor

W tej sekcji możesz odtwarzane nieco ze składnią Razor, aby w pewnym sensie podstawowej składni.

Uruchom program WebMatrix, jeśli nie jest jeszcze uruchomiona. Użyjesz witryny sieci Web utworzony w poprzednim samouczek ([pobierania uruchomiona z stron sieci Web](https://go.microsoft.com/fwlink/?LinkId=251578)). Aby otworzyć go ponownie, kliknij przycisk **witryny** i wybierz polecenie **WebPageMovies**:

![Wyświetlane opcje Otwórz witrynę i witryny wyróżnione ekranu startowego programu WebMatrix](intro-to-web-pages-programming/_static/image1.png)

Wybierz **pliki** obszaru roboczego.

Na wstążce kliknij **nowy** do utworzenia strony. Wybierz **CSHTML** i nadaj nazwę nowej strony *TestRazor.cshtml*.

Kliknij przycisk **OK**.

Skopiuj następujące do pliku, całkowicie zastępując tym, co jest dostępne.

> [!NOTE]
> Podczas kopiowania kod lub znacznik w przykładach w stronę, wcięcia i wyrównanie może nie być takie same jak samouczka. Wcięcia i wyrównanie nie wpływa na sposób kod uruchamia się, że.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Badanie przykładową stronę

Większość widoczna jest zwykłej HTML. U góry istnieje jednak ten blok kodu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Zwróć uwagę, o tym blok kodu następujących czynności:

- @-Znak informuje ASP.NET że występuje kodu Razor, HTML nie. ASP.NET będzie traktować wszystko po @-znak jako kodu do czasu jej do kod HTML zostanie ponownie uruchomione. (W tym przypadku to jest &lt;! DOCTYPE&gt; elementu.
- Nawiasy klamrowe ({i}) należy ująć bloku kodu Razor, jeśli kod ma więcej niż jeden wiersz. Nawiasy klamrowe Poinformuj ASP.NET, gdzie kodu dla tego bloku początkowej i końcowej.
- / / Znaków oznaczyć komentarz — to znaczy części kodu, który nie będzie uruchomiony.
- Każda instrukcja musi kończyć się średnikiem (;). (Nie komentarze, mimo że.)
- Można zapisać wartości w *zmienne*, które można tworzyć (*zadeklarować*) z var. — słowo kluczowe Po utworzeniu zmiennej możesz nadać jej nazwę, która może zawierać litery, cyfry i podkreślenia (\_). Nazwy zmiennych nie może zaczynać się od cyfry i nie można użyć nazwy programowania słowa kluczowego (na przykład var).
- Ciągi znaków (takich jak "ASP.NET" i "Stron sieci Web") należy ująć w cudzysłowy. (Musi być podwójny cudzysłów). Liczby nie są w znaki cudzysłowu.
- Odstępy poza znaki cudzysłowu nie ma znaczenia. Podziały wierszy przeważnie nie ma znaczenia; wyjątek jest, że nie można podzielić na ciąg w cudzysłowie na wierszy. Wcięcia i wyrównanie nie ma znaczenia.

Coś, co nie jest jasne, w tym przykładzie jest to cały kod z uwzględnieniem wielkości liter. Oznacza to, że TheSum zmiennej jest zmienną inną niż zmienne, które mogą być nazwane theSum lub thesum. Podobnie var jest słowem kluczowym, ale nie jest Var.

### <a name="objects-and-properties-and-methods"></a>Obiekty, właściwości i metod

Oznacza to, że wyrażenie DateTime.Now. Proste warunków, DateTime jest *obiektu*. Obiekt jest rzecz, którą można programu z — strona, pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta, itp. Obiekty mają co najmniej jeden *właściwości* opisują ich właściwości. Obiekt pola tekstowego ma właściwość tekst (między innymi), obiekt żądania ma właściwość Url (i inne), wiadomości e-mail ma właściwość od i do właściwości, i tak dalej. Obiekty również mieć *metody* , które są "zleceń" mogą wykonywać. Można będzie działać z obiektami partii.

Jak widać w przykładzie DateTime jest obiekt, który umożliwia program daty i godziny. Ma właściwości o nazwie teraz, która zwraca bieżącą datę i godzinę.

### <a name="using-code-to-render-markup-in-the-page"></a>Przy użyciu kodu do renderowania kodu znaczników na stronie

W treści strony zauważyć następujące czynności:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Ponownie znaku @ informuje ASP.NET tego poniżej kod, nie HTML. W znaczniku można dodać następuje wyrażenia kodu i ASP.NET spowoduje, że wartość tego wyrażenia prawa w tym momencie. W tym przykładzie @a spowoduje, że niezależnie od wartości jest zmiennej o nazwie, @product renderuje niezależnie od jest w zmiennej o nazwie produktu i tak dalej.

Nie masz ograniczone do zmiennych, jednak. W kilku przypadkach w tym miejscu znaku @ poprzedza wyrażenia:

- @(\*b) renderuje produkt jest bez względu na zmienne i b. ( \* Operator oznacza mnożenia.)
- @(technologia + "" + produktu) renderuje wartości w zmiennych technologii i produktów po ich łączenie i dodaniu odstęp między nimi. Łączenie ciągów operator (+) jest taka sama jak operator dodawania liczb. ASP.NET zwykle można określić, czy podczas pracy z numerami lub z ciągami i prawej równoznaczne z operatora +.
- @Request.Url renderuje właściwości adresu Url obiektu żądania. Obiekt żądania zawiera informacje o bieżącym żądaniu z przeglądarki, a oczywiście właściwość adres Url zawiera adres URL tego bieżącego żądania.

Przykład jest również zaprojektowana tak, że użytkownik, który można pracować na różne sposoby. Można wykonywania obliczeń w bloku kodu u góry, umieść wyniki do zmiennej i renderowania zmiennej w znaczniku. Lub możesz wykonać obliczeń w prawej strony wyrażenia w znaczniku. Podejście, którego używasz zależy od pracy oraz, w pewnym stopniu na własnych preferencji.

### <a name="seeing-the-code-in-action"></a>Wyświetlanie kodu w akcji

Kliknij prawym przyciskiem myszy nazwę pliku, a następnie wybierz pozycję **Uruchom w przeglądarce**. Zostanie wyświetlona strona w przeglądarce z wartości i wyrażeń rozwiązane na stronie.

![Uruchomiony w przeglądarce stronę "TestRazor"](intro-to-web-pages-programming/_static/image2.png)

Spójrz na źródło w przeglądarce.

!["Test Razor" źródło strony w przeglądarce](intro-to-web-pages-programming/_static/image3.png)

Zgodnie z oczekiwaniami ze środowiska w poprzednich instrukcji, żaden z kodu Razor nie jest na stronie. Wszystkie dostępne są wartości wyświetlane rzeczywistych. Po uruchomieniu strony faktycznie wprowadzasz żądanie do serwera sieci web, które są wbudowane w program WebMatrix. Po odebraniu żądania ASP.NET rozpoznaje wszystkie wartości i wyrażeń i renderuje ich wartości na stronie. Wysyła następnie strony w przeglądarce.

> [!TIP] 
> 
> **Razor i C#**
> 
> Do chwili zostały możemy powiedzieć pracujesz o składni Razor. Jest to istotne, ale nie jest pełną wątku. Rzeczywiste język programowania używany jest nazywany *C#*. C# został utworzony przez firmę Microsoft za pośrednictwem dekadę temu i stało się jednym z głównej języków programowania do tworzenia aplikacji systemu Windows. Wszystkie reguły, które zostały przeczytane dotyczące nazwy zmiennej i utworzyć instrukcje i tak dalej są faktycznie wszystkich reguł języka C#.
> 
> Razor w szczególności dotyczy niewielki zestaw konwencjach jak osadzić ten kod do strony. Na przykład Konwencji przy użyciu, aby oznaczyć kodu na stronie i @ {} do osadzenia blok kodu jest elementem Razor strony. Pomocnicy również są uważane za część Razor. Składnia razor jest używana w większej liczbie miejsc niż tylko w programie ASP.NET Web Pages. (Na przykład, jest on w także widoki ASP.NET MVC.)
> 
> Firma Microsoft informacje o tym ponieważ jeśli szukasz informacji na temat programowania stron sieci Web platformy ASP.NET można znaleźć wiele odwołań do elementu Razor. Jednak wiele te odwołania nie dotyczą co zrobić w przypadku i dlatego może być mylące. A w rzeczywistości wiele pytań programowania naprawdę mają zostać o pracy w języku C# albo pracy z programem ASP.NET. Dlatego podczas przeglądania w szczególności informacji na temat Razor, nie może być potrzebne odpowiedzi.


## <a name="adding-some-conditional-logic"></a>Dodanie niektórych logikę warunkową

Jednym z najważniejszych funkcji o przy użyciu kodu na stronie jest, można zmienić, co się dzieje na podstawie w różnych warunkach. W tej części samouczka będzie odtwarzać z kilka sposobów, aby zmienić informacje wyświetlane na stronie.

Przykład będzie prosty i nieco contrived tak, aby firma Microsoft może skupić się na logikę warunkową. Strona, którą utworzysz będzie wykonaj następujące czynności:

- Pokaż inny tekst na stronie, w zależności od tego, czy jest po raz pierwszy ta strona jest wyświetlana, lub czy został kliknięty przycisk przesłać strony. Który będzie stanowić pierwszy test warunkowy.
- Wyświetla komunikat tylko wtedy, gdy wartość jest przekazywany w ciągu kwerendy adresu URL (http://...?show=true). Który będzie stanowić drugi test warunkowy.

W programie WebMatrix, Utwórz stronę i nadaj mu nazwę *TestRazorPart2.cshtml*. (Na wstążce kliknij **nowy**, wybierz **CSHTML**, nazwę pliku, a następnie kliknij przycisk **OK**.)

Zamień zawartość tej strony następujące czynności:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Blok kodu u góry inicjuje zmiennej o nazwie wiadomości z tekstem. W treści strony zawartość zmiennej wiadomości są wyświetlane wewnątrz &lt;p&gt; elementu. Zawiera także znaczników &lt;wejściowych&gt; element, aby utworzyć **przesyłania** przycisku.

Uruchomić stronę, aby zobaczyć, jak działa teraz. Obecnie jest zasadniczo strony statyczne, nawet jeśli klikniesz przycisk **przesyłania** przycisku.

Wróć do programu WebMatrix. Wewnątrz bloku kodu, Dodaj następujący wyróżniony kod *po* wiersza, który inicjuje komunikat:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Jeżeli blok {}

Co dodaną został if warunku. W kodzie Jeśli warunek ma strukturę następująco:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Warunek do sprawdzenia jest w nawiasach. Musi to być wartość lub wyrażenie, które zwraca wartość PRAWDA lub FAŁSZ. Jeśli warunek jest spełniony, ASP.NET działa instrukcji lub instrukcji, które są w nawiasach klamrowych. (Są to *następnie* częścią *Jeżeli to* logikę.) Jeśli warunek nie jest spełniony, zostanie pominięty bloku kodu.

Oto kilka przykładów warunki, można sprawdzić w przypadku instrukcji:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Zmienne względem wartości lub wyrażenia można testować przy użyciu <em>operatora logicznego</em> lub <em>operator porównania</em>: równe (==), przekracza (&gt;), poniżej (&lt;), większe lub równe (&gt;=) i mniejsze niż lub równe (&lt;=). ! = Oznacza operator nie jest równa — na przykład, jeśli (! = 0) oznacza <em>Jeśli</em> <em>a</em><em>nie jest równa 0</em>.

> [!NOTE]
> Upewnij się, że można zauważyć, że operator porównania dla równa (==) nie jest taka sama jak =. = — Operator służy tylko do przypisywania wartości (var = 2). Jeśli tych operatorów należy pomylone, albo zostanie wyświetlony błąd lub niektóre dziwne wyniki.


Aby sprawdzić, czy element ma wartość true, jest Pełna składnia if(IsDone == true). Ale można również użyć if(IsDone) skrótów. Jeśli nie żaden operator porównania, ASP.NET zakłada testujesz na wartość true.

! Operator samodzielnie oznacza logiczne nie. Na przykład warunek if (! Oznacza, że IsPost) *Jeśli IsPost nie zostanie spełniony*.

Warunki można połączyć za pomocą logiczny AND (&amp; &amp; operatora) lub operatora logicznego OR (|| — operator). Na przykład ostatnie, jeśli warunki w poprzednim oznacza przykłady *Jeśli FileProcessingIsDone nie jest ustawiony na wartość true i displayMessage ma wartość false*.

### <a name="the-else-block"></a>Else bloku

Jedyną operacją końcowego o tym, czy bloki: Jeśli blok po bloku else. Przydaje się innego bloku jest konieczne wykonanie innego kodu, gdy warunek ma wartość false. Poniżej przedstawiono prosty przykład:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Zobaczysz kilka przykładów w kolejnych samouczkach z tej serii, gdzie jest przydatna przy użyciu innego bloku.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Sprawdzenie, czy żądanie jest przesyłania (post)

Istnieje więcej, ale teraz wrócić do przykładu, którego warunku if(IsPost) {...}. IsPost jest rzeczywiście właściwości bieżącej strony. Po raz pierwszy żąda strony, IsPost zwraca wartość false. Jednak jeśli kliknięcie przycisku lub w przeciwnym razie przesłanie strony — to znaczy po — IsPost zwraca wartość true. Dlatego IsPost pozwala określić, czy jest zajmujących przesyłania formularza. (Pod względem zleceń HTTP, jeśli żądanie jest operację GET IsPost zwraca wartość false. Jeśli żądanie jest operację POST, IsPost zwraca wartość true). W samouczku nowsze będzie współpracować zwykłe formularze, w którym ten test staje się szczególnie przydatne.

Uruchom strony. Ponieważ jest to po raz pierwszy jest żądanie strony widać "Jest już żądanej strony po raz pierwszy". Ten ciąg jest wartość, która zainicjowana zmienna wiadomości. Brak testu if(IsPost), ale zwraca wartość false w tej chwili, tak aby blokował kodu wewnątrz if nie działa.

Kliknij przycisk **przesyłania** przycisku. Ponownie zostanie zażądana strona. Jako przed, zmiennej komunikatu jest równa "Jest to pierwsza...". Ale tym razem if(IsPost) testu zwraca wartość true, tak kodu wewnątrz if blokowanie działa. Kod zmienia się wartość zmiennej wiadomości na inną wartość, czyli, co jest renderowany w znaczniku.

Teraz Dodaj if warunku w znaczniku. Poniżej &lt;p&gt; element, który zawiera **przesyłania** przycisku, Dodaj następujący kod:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

W przypadku dodawania kodu w znaczniku, dlatego należy rozpocząć @. A jeśli test podobnej do tej, dodano wcześniej w bloku kodu. W nawiasach klamrowych, jednak w przypadku dodawania zwykłej HTML — co najmniej jest zwykłej dopóki nie zostanie dostarczona do @DateTime.Now. Jest to inny niewielki kodu Razor, więc ponownie należy dodać przed nim.

Punkt, w tym miejscu jest dodaniem Jeśli warunki zarówno kod zablokować u góry, a w znaczniku. Jeśli używasz if warunek w treści strony wierszy wewnątrz bloku może być znaczników lub kodu. W takim przypadku i podobnie jak w dowolnym momencie można mieszać znaczników i kodu, należy użyć, aby był wyczyść dla platformy ASP.NET gdy kod jest.

Uruchom strony, a następnie kliknij przycisk **przesyłania**. Teraz można nie tylko komunikat różnych przesłać ("teraz po przesłaniu..."), ale zostanie wyświetlony nowy komunikat, który wyświetla datę i godzinę.

![Strona "Test Razor 2" uruchomiony w przeglądarce z sygnaturą czasową wyświetlanie po przesyłania](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testowanie wartości ciągu zapytania

Co więcej testów. Teraz, należy dodać w przypadku bloku, który umożliwia porównanie wartości o nazwie show, które mogą być przekazywane w ciągu zapytania. (Podobnie do następującej: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) stronie zostanie zmieniona tak, aby wiadomość możesz już został wyświetlanie ("Jest to pierwsza..." itp.) jest wyświetlany tylko wtedy, jeśli wartość Pokaż ma wartość true.

W dolnej (ale wewnątrz) blok kodu w górnej części strony, Dodaj następujące informacje:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Kompletny kod wygląd teraz bloku jak w następującym przykładzie. (Należy pamiętać, że po skopiowaniu kodu na stronie wcięcie może wyglądać inaczej. Ale który nie ma wpływu na sposób uruchamiania kodu).

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Nowy kod w bloku inicjuje zmiennej o nazwie showMessage na wartość false. Następnie wykonuje if testu do wyszukiwania wartości w ciągu zapytania. Po pierwsze żądanie strony, ma adres URL podobne do następującego:

`http://localhost:43097/TestRazorPart2.cshtml`

Kod określa, czy adres URL zawiera zmienną o nazwie show w ciągu zapytania, podobnie jak w tej wersji adresu URL:

`http://localhost:43097/TestRazorPart2.cshtml`? Pokaż = true

Badanie analizuje właściwość QueryString obiektu żądania. Jeśli ciąg zapytania zawiera element o nazwie show, a jeśli element ma wartość true, jeśli blok uruchamia i ustawia zmienną showMessage na wartość true.

Jak widać jest lewy w tym miejscu. Takie jak nazwa mówi, ciąg zapytania jest ciąg. Jednak można tylko sprawdzić, true i false, jeśli wartość testowany jest wartość logiczna (PRAWDA/FAŁSZ). Przed wartość zmiennej Pokaż można sprawdzić w ciągu zapytania, należy przekonwertować go na wartość logiczną. To jest metoda AsBool — przyjmuje ciągu na wejściu i konwertuje ją na wartość logiczną. Wyraźnie widać Jeśli ten ciąg ma wartość "prawda", metoda AsBool konwertuje tę wartość na true. Jeśli wartość ciągu jest inny, AsBool zwraca wartość false.

> [!TIP] 
> 
> **Typy danych i metod As()**
> 
> Firma Microsoft już tylko powiedział wykonanej do tej pory podczas tworzenia zmiennej użycie var. — słowo kluczowe To nie jest cały artykuł, mimo że. W celu manipulowania wartości — dodawanie liczb, lub ciągów, lub porównanie dat lub test na wartość true, false — C# ma do pracy z reprezentacji wewnętrznej odpowiednie wartości. C# można *zwykle* dowiedzieć się, jakie powinny być reprezentacja tego (oznacza to, co *typu* dane) oparte na wykonywanych przy użyciu wartości. Teraz, a następnie jednak nie który. Jeśli nie ma pomagać jawnie wskazując jak C# powinno reprezentować danych. Metoda AsBool robi to — informuje C#, czy wartość ciągu "true" lub "false" powinien być traktowany jako wartość logiczną. Istnieją podobne metody reprezentują ciągi jako innych typów, a także jak AsInt (Traktuj jako liczba całkowita), AsDateTime (Traktuj jako data/godzina), AsFloat (Traktuj jako liczba zmiennoprzecinkowa) i tak dalej. Korzystając z tych metod (), jeśli C# nie może reprezentować wartość ciągu zgodnie z wymaganiami, zostanie wyświetlony błąd.


W znaczniku strony, usuń lub komentarz tego elementu (w tym miejscu jego znajduje się poza komentarzem):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Po prawej, gdzie została usunięta lub oznaczone jako komentarz tekstu, Dodaj następujący kod:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Jeśli test który renderowania, gdy ma wartość true, zmienna showMessage &lt;p&gt; elementu o wartości zmiennej wiadomości.

### <a name="summary-of-your-conditional-logic"></a>Podsumowanie logiki warunkowe

W przypadku, gdy nie masz całkowicie pewności, co użytkownik został właśnie zrobić, w tym miejscu znajduje się podsumowanie.

- Zmienna wiadomości jest ustawiana na domyślny ciąg ("jest pierwszym...").
- Jeśli żądanie dostępu do strony jest wynikiem przesyłania (post), wartość komunikatu zostanie zmieniona na "teraz po przesłaniu..."
- Zmienna showMessage jest ustawiana na wartość false.
- Jeśli ciąg zapytania zawiera? Pokaż = true, zmienna showMessage jest ustawiona na true.
- W znaczniku, jeśli ma wartość true, showMessage &lt;p&gt; element jest renderowany pokazujący wartość komunikatu. (Jeśli showMessage ma wartość false, nic nie jest renderowany w tym momencie w znaczniku.)
- W znaczniku, jeśli żądanie jest żądaniem post &lt;p&gt; element jest renderowany, który wyświetla datę i godzinę.

Uruchom strony. Nie ma, ponieważ showMessage jest wartość FAŁSZ, więc w znaczniku testu if(showMessage) zwraca wartość false.

Kliknij przycisk **przesłać**. Zostanie wyświetlony daty i godziny, ale nadal żaden komunikat.

W przeglądarce przejdź do pola adresu URL i dodaj następującą na końcu adresu URL:? Pokaż = true, a następnie naciśnij klawisz Enter.

!["Test Razor 2" strony w przeglądarka wyświetlająca ciąg zapytania](intro-to-web-pages-programming/_static/image5.png)

Ta strona jest wyświetlana ponownie. (Ponieważ adres URL jest zmieniony, jest nowe żądanie, nie Prześlij). Kliknij przycisk **przesyłania** ponownie. Ten komunikat jest wyświetlany ponownie, ponieważ jest data i godzina.

!["Test Razor 2" strony po przesyłania w przypadku ciągu zapytania](intro-to-web-pages-programming/_static/image6.png)

W adresie URL Zmień? Pokaż = true na? Pokaż = false, a następnie naciśnij klawisz Enter. Ponownie wysłać stronę. Strona jest do sposobu uruchamiania — Brak komunikatu.

Jak wspomniano wcześniej, logikę w tym przykładzie jest trochę contrived. Jednakże, jeżeli będzie znaleziona w wielu stron, i potrwa co najmniej jednego z formularzy w tym samouczku tutaj.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalowanie pomocnika (wyświetlanie obrazów Gravatar)

Niektóre zadania, które osoby często mają być wykonane na stronach sieci web wymaga dużo kodu lub wymagają dodatkowych wiedzy. Przykłady: wyświetlanie wykresu dla danych. umieszczanie Facebook "Jak" przycisk na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie lub zmiana rozmiaru obrazów; przy użyciu usługi PayPal dla witryny. Aby ułatwić czy rodzaju rzeczy, ASP.NET Web Pages umożliwia używanie *pomocników*. Pomocnicy są składniki instalowanej lokacji i umożliwiające wykonywanie typowych zadań przy użyciu kilku wierszy kodu Razor.

Strony ASP.NET Web Pages ma kilka wątków wbudowane. Wiele wątków są jednak dostępne w pakietach (dodatki), które znajdują się za pomocą Menedżera pakietów NuGet. NuGet pozwala wybrać pakiet do zainstalowania, a następnie dba o szczegóły instalacji.

W tej części samouczka nastąpi instalacja pomocnika, który umożliwia wyświetlanie obrazów Gravatar ("globalnie rozpoznanym awatara"). Dowiesz się dwie czynności. Jedna jest jak Znajdź i zainstaluj obiekt pomocnika. Dowiesz się, jak pomocnika ułatwia czymś, które w przeciwnym razie trzeba wykonać, korzystając z dużej ilości kodu będzie musiał zapisać samodzielnie.

Możesz zarejestrować własne Gravatar w witrynie Gravatar w [ http://www.gravatar.com/ ](http://www.gravatar.com/), ale nie jest istotne, aby utworzyć konto Gravatar do wykonania tej części samouczka.

W programie WebMatrix, kliknij przycisk **NuGet** przycisku.

![Okno dialogowe galerii NuGet w programie WebMatrix](intro-to-web-pages-programming/_static/image7.png)

To spowoduje uruchomienie Menedżera pakietów NuGet i wyświetlenie dostępnych pakietów. (Nie wszystkie pakiety są pomocnicy; dodać niektóre funkcje do programu WebMatrix się niektóre są dodatkowe szablony itd.) Może wystąpić komunikat o błędzie dotyczący niezgodności wersji. Możesz zignorować ten komunikat o błędzie, klikając **OK** i postępowanie z tego samouczka.

![Okno dialogowe galerii NuGet w programie WebMatrix](intro-to-web-pages-programming/_static/image8.png)

W polu wyszukiwania wprowadź "pomocników platformy asp.net". NuGet wyświetlane pakiety zgodne z warunkami wyszukiwania.

![Galeria NuGet w programie WebMatrix przedstawiający pakietów](intro-to-web-pages-programming/_static/image9.png)

Biblioteka pomocników platformy ASP.NET sieci Web zawiera kod, aby uprościć wiele typowych zadań, łącznie z użyciem Gravatar obrazów. Wybierz **bibliotekę pomocników platformy ASP.NET Web** pakiet, a następnie kliknij przycisk **zainstalować** można uruchomić Instalatora. Wybierz **tak** po otrzymaniu monitu, jeśli chcesz zainstalować pakiet i zaakceptuj warunki, aby zakończyć instalację.

To wszystko. NuGet pobiera i instaluje wszystkim łącznie z dodatkowych składników, które mogą być wymagane (*zależności*).

Jeśli zaistnieje należy odinstalować pomocnika, proces jest bardzo podobne. Kliknij przycisk **NuGet** , kliknij **zainstalowana** , a następnie wybierz pakiet, którego chcesz odinstalować.

## <a name="using-a-helper-in-a-page"></a>Przy użyciu pomocnika, na stronie

Teraz użyjesz pomocnika, który został właśnie zainstalowany. Proces dodawania pomocnika do strony jest podobny dla większości pomocników.

W programie WebMatrix, Utwórz stronę i nadaj mu nazwę *GravatarTest.cshml*. (Tworzysz specjalną stroną, aby przetestować pomocnika, ale pomocników można użyć w dowolnej strony w witrynie).

Wewnątrz &lt;treści&gt; elementu, Dodaj &lt;div&gt; elementu. Wewnątrz &lt;div&gt; elementu, wpisz to:

@Gravatar.

@-Znak jest ten sam znak korzystasz do oznaczania kodu Razor. **Gravatar** obiekt pomocnika, w którym pracujesz.

Gdy użytkownik wpisze kropkę (.), program WebMatrix Wyświetla listę *metody* (funkcje) udostępnia pomocnika Gravatar:

![Listy rozwijanej IntelliSense pomocnika Gravatar](intro-to-web-pages-programming/_static/image10.png)

Funkcja ta jest określana jako *IntelliSense*. Pomaga programowanie podając odpowiednie kontekstu wyborów. IntelliSense współpracuje z HTML, CSS, kod platformy ASP.NET, JavaScript i innych języków, które są obsługiwane w programie WebMatrix. Jest innej funkcji, która ułatwia tworzenie stron sieci web w programie WebMatrix.

Naciśnij G na klawiaturze, zobacz, czy IntelliSense znajduje metodę GetHtml. Naciśnij klawisz Tab. IntelliSense wstawia wybranej metody (GetHtml). Wpisz nawias otwierający i zwróć uwagę, zamykający nawias okrągły zostanie automatycznie dodany. Wpisz swój adres e-mail w znaki cudzysłowu między dwa nawiasy. Jeśli masz konto Gravatar, zostanie zwrócony obraz profilu. Jeśli nie masz konta Gravatar, jest zwracany domyślny obraz. Gdy wszystko będzie gotowe, wiersz wygląda następująco:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Teraz można wyświetlić strony w przeglądarce. Obraz lub domyślnego obrazu jest wyświetlana w zależności od tego, czy masz konto Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![domyślny obraz](intro-to-web-pages-programming/_static/image12.png)

Aby uzyskać informacje o tym, co pomocnika zadań dla Ciebie, Wyświetl źródło strony w przeglądarce. Wraz z HTML, które były dostępne na stronie zostanie wyświetlony element obrazu, który zawiera identyfikator. To jest kod, który pomocnika renderowany na stronie w miejscu, gdzie ma @Gravatar.GetHtml. Pomocnik trwało informacje dostarczane i wygenerowany kod, który komunikuje się bezpośrednio do Gravatar Aby odzyskać prawidłowy obraz dla podane konto.

Metoda GetHtml umożliwia również dostosować obraz podając innych parametrów. Poniższy kod przedstawia sposób żądania obraz ma szerokości i wysokości 40 pikseli oraz że używa określonego domyślnego obrazu o nazwie **wavatar** Jeśli określone konto nie istnieje.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Ten kod tworzy przypominać następujące wyniki (domyślny obraz losowo różnią się).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Powtarzający się dalej

Aby zachować ten samouczek krótki, było skupić się na tylko kilka podstawowych. Oczywiście istnieje *partii* więcej Razor i C#. Dowiesz się więcej jako użytkownik przechodzi przez te samouczki. Jeśli chcesz dowiedzieć się więcej na temat programowania aspektów Razor i C# od razu, można znaleźć tutaj bardziej szczegółowego wprowadzenie: [wprowadzenie do platformy ASP.NET Web programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

Następny samouczek stanowi wprowadzenie do pracy z bazą danych. W tym samouczku będzie rozpocząć tworzenie przykładowej aplikacji, która pozwala na liście ulubionych filmów.

## <a name="complete-listing-for-testrazor-page"></a>Pełna lista TestRazor strony

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Pełna lista TestRazorPart2 strony

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Pełna lista GravatarTest strony

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Pomocnik usługi Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Poprzednie](getting-started.md)
> [dalej](displaying-data.md)
