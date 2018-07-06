---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (C#) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym rozdziale umożliwia przegląd programowania przy użyciu stron ASP.NET Web Pages z użyciem składni Razor. ASP.NET to technologia firmy Microsoft dotyczące uruchamiania dynamicznych sieci Web...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 22985b71d39d93b8ad42ee923b872846f2fafa99
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814304"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (C#)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule umożliwia przegląd programowania przy użyciu stron ASP.NET Web Pages z użyciem składni Razor. ASP.NET to technologia firmy Microsoft dotyczące uruchamiania dynamicznych stron sieci web na serwerach sieci web. To zespoły artykuły na temat korzystania z języka programowania C#.
> 
> **Dowiesz się**:
> 
> - Najważniejsze 8, porady dotyczące wprowadzenie do programowania stron sieci Web platformy ASP.NET używająca składni Razor programowania.
> - Podstawowe pojęcia programowania, które będą potrzebne.
> - Jaki kod serwera programu ASP.NET o składni Razor są wszystkie informacje.
>   
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Najważniejsze 8 porady dotyczące programowania

W tej sekcji przedstawiono kilka wskazówek, które bezwzględnie musisz wiedzieć, po rozpoczęciu pisania kodu serwera ASP.NET używająca składni Razor.

> [!NOTE]
> Składnia Razor zależy od języka programowania C#, a to język, który jest używany w większości przypadków przy użyciu stron ASP.NET Web Pages. Jednak składni Razor obsługuje również języka Visual Basic i wszystko, co widać, że możesz również wykonać w języku Visual Basic. Aby uzyskać więcej informacji, zobacz dodatku [języka Visual Basic i składni](https://go.microsoft.com/fwlink/?LinkId=202908).


Więcej informacji na temat większości z tych technik programowania można znaleźć w dalszej części tego artykułu.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Możesz dodać kod do strony przy użyciu znaku @

`@` Znak rozpoczyna się w tekście wyrażeń, bloków pojedynczej instrukcji i bloków wielu instrukcji:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Jest to, te instrukcje wyglądać po uruchomieniu na stronie w przeglądarce:

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Kodowanie HTML**
> 
> Podczas wyświetlania zawartości na stronie za pomocą `@` znaków, jak w poprzednich przykładach, ASP.NET koduje jako HTML dane wyjściowe. Spowoduje to zastąpienie zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) przy użyciu kodów, umożliwiające znaków, które mają być wyświetlane jako znaki na stronie sieci web, a interpretowany jako tagów HTML lub jednostki. Zakodowany w formacie HTML, dane wyjściowe z kodu serwera mogą być wyświetlane nieprawidłowo i spowodować narażenie na zagrożenia bezpieczeństwa strony.
> 
> Jeśli dowiesz się, jak dane wyjściowe kod znaczników HTML, który renderuje tagi jako kod znaczników (na przykład `<p></p>` akapitu lub `<em></em>` aby wyróżnić tekst), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu](#BM_CombiningTextMarkupAndCode) w dalszej części tego artykułu.
> 
> Możesz dowiedzieć się więcej o kodowanie HTML w [Praca z formularzami](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Bloki kodu są ująć w nawiasy klamrowe

A *blok kodu* zawiera jedną lub więcej instrukcji kodu i jest ujęty w nawiasy klamrowe.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Wewnątrz bloku zakończenia każda instrukcja kodu przy użyciu średnika

Wewnątrz bloku kodu każda instrukcja kompletny kod musi być zakończona średnikiem. Wbudowane wyrażenia nie kończy się średnikiem.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. W przypadku używania zmiennych do przechowywania wartości

Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty, itp. Możesz utworzyć nową zmienną za pomocą `var` — słowo kluczowe. Wartości zmiennych można wstawić bezpośrednio na stronie za pomocą `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Ujmij wartości literału ciągu w znaki podwójnego cudzysłowu

A *ciąg* jest sekwencją znaków, które są traktowane jako tekst. Aby określić ciąg, ująć w znaki cudzysłowu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Jeśli ciąg, który chcesz wyświetlić zawiera znak ukośnika odwrotnego ( `\` ) lub podwójny cudzysłów ( `"` ), użyj *verbatim literału ciągu* , jest poprzedzony znakiem `@` operatora. (W języku C#, \ znak ma specjalne znaczenie, chyba że używasz verbatim literału ciągu.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Aby osadzić podwójne znaki cudzysłowu, użyć literału ciągu verbatim i powtórz znaków cudzysłowu:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

W tym miejscu jest wynikiem użycia obu tych przykładów na stronie:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Należy zauważyć, że `@` znak jest używany zarówno do oznaczenia verbatim literałów w języku C#, jak i do oznaczania kodu na stronach ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Kod jest uwzględniana wielkość liter

W języku C# słowa kluczowe (takie jak `var`, `true`, i `if`) i w nazwach zmiennych jest rozróżniana wielkość liter. Następujące wiersze kodu tworzą dwie różne zmienne, `lastName` i `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Jeśli zadeklarujesz zmienną `var lastName = "Smith";` i jeśli zostanie podjęta próba odwoływać się do tej zmiennej na stronie jako `@LastName`, powoduje błąd, ponieważ `LastName` nie zostanie rozpoznane.

> [!NOTE]
> W języku Visual Basic słowa kluczowe i zmienne są *nie* z uwzględnieniem wielkości liter.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Obejmuje większość kodowania obiektów

*Obiektu* reprezentuje rzecz, którą można programować za pomocą &#8212; strony pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta (wiersz bazy danych), itp. Obiekty mają właściwości, które opisują ich właściwości i może czytać lub zmieniać &#8212; ma obiekt pola tekstowego `Text` właściwości (między innymi), obiekt żądania ma `Url` właściwość, wiadomość e-mail ma `From` właściwości i obiekt klienta ma `FirstName` właściwości. Obiektów ma także metody, które są &quot;zleceń&quot; mogą wykonywać. Przykłady obejmują obiekt pliku `Save` metodę, obiekt obrazu `Rotate` metody i obiekt e-mail `Send` metody.

Często będziesz pracować `Request` obiektów, które zapewnia informacje, takie jak wartości pól tekstowych (pola formularza) na stronie, jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp. Poniższy przykład pokazuje, jak uzyskiwanie dostępu do właściwości `Request` obiektu i wywoływania `MapPath` metody `Request` obiektu, który zapewnia ścieżkę bezwzględną strony na serwerze:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Można napisać kod, który podejmuje decyzje

Kluczową funkcją dynamicznych stron sieci web jest, czy można określić, co należy zrobić, na podstawie warunków. Jest najbardziej popularny sposób, w tym celu `if` — instrukcja (i opcjonalnie `else` instrukcji).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Wykonywanie instrukcji `if(IsPost)` jest skrót sposobem pisania `if(IsPost == true)`. Wraz z `if` instrukcji, istnieje wiele sposobów, aby przetestować warunki, powtórz bloki kodu, i itd., które są opisane w dalszej części tego artykułu.

Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **przesyłania**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET i POST metod i właściwości IsPost
> 
> Protokół używany dla stron sieci web (HTTP) obsługuje bardzo ograniczoną liczbę metod (poleceń), które są używane na wysyłanie żądań do serwera. Dwie najbardziej typowe to GET, który służy do odczytu strony i WPIS, który jest używany do przesyłania strony. Ogólnie rzecz biorąc po raz pierwszy użytkownik zgłasza żądanie strony, strony jest przesyłane przy użyciu GET. Jeśli użytkownik wypełnia formularz, a następnie kliknie przycisk przesyłania, przeglądarki wysyła żądanie POST do serwera.
> 
> W programowanie dla sieci web jest często grupowaniu można sprawdzić, czy strona jest wymagana jako GET lub POST, aby znać sposób przetwarzania tej strony. W składniku ASP.NET Web Pages można użyć `IsPost` właściwość, aby sprawdzić, czy żądanie GET lub POST. Jeśli żądanie jest żądaniem POST `IsPost` właściwość zostanie zwrócona wartość PRAWDA, a można wykonywać takie czynności, takich jak odczyt wartości pola tekstowe w formularzu. Wiele przykładów zobaczysz pokazują, jak można przetworzyć strony inaczej w zależności od wartości `IsPost`.


## <a name="a-simple-code-example"></a>Prosty przykład kodu

Ta procedura pokazuje, jak utworzyć stronę, która przedstawia podstawowe techniki programowania. W tym przykładzie utworzysz stronę, która umożliwia użytkownikom wprowadź dwie liczby, a następnie dodanie ich i wyświetla wynik.

1. W edytorze, Utwórz nowy plik i nadaj mu nazwę *AddNumbers.cshtml*.
2. Skopiuj następujący kod i znaczników do strony, zastępując wszystko już na stronie.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Oto kilka rzeczy, umożliwiające należy zwrócić uwagę:

    - `@` Znak rozpoczyna się pierwszego bloku kodu na stronie i poprzedza ono `totalMessage` zmiennej, która jest osadzony w dolnej części strony.
    - Blok, w górnej części strony jest ujęty w nawiasy klamrowe.
    - W bloku u góry wszystkie wiersze kończyć średnikiem.
    - Zmienne `total`, `num1`, `num2`, i `totalMessage` przechowywać kilka liczb i ciąg.
    - Wartość literału ciągu przypisana do `totalMessage` zmiennej znajduje się w znaki podwójnego cudzysłowu.
    - Ponieważ kod jest rozróżniana wielkość liter, kiedy `totalMessage` zmienna jest używana w dolnej części strony, jego nazwa musi dokładnie pasować zmiennej u góry.
    - Wyrażenie `num1.AsInt() + num2.AsInt()` pokazuje, jak pracować z obiektami i metody. `AsInt` Metody dla każdej zmiennej konwertuje ciąg wprowadzanego przez użytkownika numer (liczba całkowita) tak, aby wykonywać operacje arytmetyczne na nim.
    - `<form>` Znacznik obejmuje `method="post"` atrybutu. Określa to, że gdy użytkownik kliknie **Dodaj**, strony, które będą wysyłane do serwera przy użyciu metody POST protokołu HTTP. Po przesłaniu strony `if(IsPost)` testu daje w wyniku wartość PRAWDA, a warunkowy kod działa, wyświetlanie wynikiem dodania liczb.
3. Zapisz stronę i uruchom go w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Wprowadź dwoma liczbami całkowitymi, a następnie kliknij przycisk **Dodaj** przycisku. 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Podstawowe pojęcia związane z programowaniem

Ten artykuł zawiera omówienie programowania dla sieci web platformy ASP.NET. Nie jest wyczerpująca badania, po prostu krótkiego przewodnika za pośrednictwem pojęcia dotyczące programowania, które będą używane najczęściej. Nawet w takim przypadku obejmują prawie wszystko, które będą potrzebne, aby rozpocząć pracę przy użyciu stron ASP.NET Web Pages.

Ale pierwszym, nieco wiedzę techniczną.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Składnia Razor, kod serwera i programu ASP.NET

Składnia razor jest prostą składnię programowania do osadzania kodu na serwerze, na stronie sieci web. Na stronie sieci web, która używa składni Razor, istnieją dwa rodzaje zawartości: Kod klienta zawartości i serwera. Zawartość klienta jest rzeczy użyto na stronach sieci web: kod znaczników HTML (elementy), informacje, takie jak CSS, stylu może niektóre skrypt po stronie klienta, takich jak JavaScript i zwykłego tekstu.

Składnia razor pozwala dodać kod serwera do tej zawartości klienta. Jeśli na stronie znajduje się kod serwera, na serwerze działa ten kod najpierw przed wysłaniem strony do przeglądarki. Uruchamiając na serwerze, kod może wykonać zadania, które mogą być o wiele bardziej złożone, aby zrobić za pomocą samodzielnie, takich jak uzyskiwanie dostępu do serwera bazy danych zawartości klienta. Co najważniejsze, kod serwera dynamicznie tworzyć zawartość klienta &#8212; można wygenerować kod znaczników HTML lub inną zawartość na bieżąco i wysłać go do przeglądarki, wraz z statyczny kod HTML, który może zawierać strony. Z perspektywy przeglądarki nie różni się od innych zawartości klienta jest zawartości klienta, który jest generowany przez kod serwera. Jak możesz już możliwość przekonania się, kod serwera, który jest wymagany jest bardzo proste.

Strony sieci web ASP.NET zawierających składnię Razor z rozszerzeniem pliku specjalne (*.cshtml* lub *.vbhtml*). Serwer rozpoznaje te rozszerzenia, uruchamia kod, który jest oznaczony przy użyciu składni Razor, a następnie wysyła strony do przeglądarki.

### <a name="where-does-aspnet-fit-in"></a>Gdy program ASP.NET pasują do

Składnia razor jest oparty na technologii firmy Microsoft o nazwie ASP.NET, która z kolei zależy od Microsoft .NET Framework. Środowiska.NET Framework to dużych, kompleksowe środowisko programowania firmy Microsoft do tworzenia praktycznie dowolnego typu aplikacji na komputerze. ASP.NET jest częścią programu .NET Framework, który jest specjalnie przeznaczony do tworzenia aplikacji sieci web. Deweloperzy używanych platformy ASP.NET można tworzyć wiele witryn sieci Web z najwyższą ruchu i największą na świecie. (Dowolnej chwili wyświetlić rozszerzenie nazwy pliku *.aspx* jako część adresu URL w witrynie, będziesz wiedzieć, że witryna została opracowana za pomocą programu ASP.NET.)

Składnia Razor zapewnia wszystkich możliwości platformy ASP.NET, ale przy użyciu uproszczoną skłądnią, która jest łatwiejsze dowiedzieć się, czy jesteś początkującym użytkownikiem, który udostępnia możesz zwiększyć produktywność gdy jesteś ekspertem. Mimo że ta składnia jest łatwa w użyciu, jego relację rodziny na platformie ASP.NET i .NET Framework oznacza, że w miarę bardziej zaawansowanych witryn sieci Web trzeba możliwości większych struktur, dostępne dla Ciebie.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Klasy i wystąpienia**
> 
> Obiekty, które z kolei są oparte na pomysł klasy korzysta z kodu serwera ASP.NET. Klasa jest definicja lub szablon dla obiektu. Na przykład aplikacja może zawierać `Customer` klasę, która definiuje właściwości i metody, których potrzebuje dowolnego obiektu klienta.
> 
> Aplikacja musi pracować z informacjami o klientach rzeczywiste, tworzy wystąpienie (lub *tworzy*) obiektu klienta. Poszczególnych klientów są osobne wystąpienie `Customer` klasy. Każde wystąpienie obsługuje te same właściwości i metody, ale dla każdego wystąpienia wartości właściwości różnią się zwykle, ponieważ każdy obiekt klienta jest unikatowa. W obiekcie jednego klienta `LastName` właściwość może być "Smith"; w innym obiekcie klienta, `LastName` właściwość może być "Kowalski".
> 
> Podobnie, dowolnej poszczególne strony sieci web w lokacji jest `Page` obiekt, który jest wystąpieniem `Page` klasy. Przycisk na stronie jest `Button` obiekt, który jest wystąpieniem `Button` klasy i tak dalej. Każde wystąpienie ma własne charakterystyki, ale wszystkie one są oparte na został określony w definicji klasy obiektu.


## <a name="basic-syntax"></a>Podstawowa składnia

Wcześniej, jak działa przykład podstawowy sposób tworzenia stron ASP.NET Web Pages i jak możesz dodać kod serwera do kod znaczników HTML. Tutaj dowiesz się podstaw pisania kodu serwera ASP.NET używająca składni Razor &#8212; czyli programowania reguły języka.

Jeśli masz doświadczenie w pracy z programowania (zwłaszcza, jeśli używano C, C++, C#, Visual Basic lub JavaScript), większość tutaj przeczytaj będą niczym nowym. Prawdopodobnie należy zapoznać się tylko z jak kod serwera jest dodawany do znaczników w *.cshtml* plików.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Łączenie tekstu, znaczników i kodu w blokach kodu

W blokach kodu serwera często zachodzi potrzeba danych wyjściowych tekst lub znaczników (lub obie) do strony. Jeśli blok kodu serwera zawiera tekst nie jest kodem, który zamiast tego powinien być renderowany jako jest, ASP.NET musi być w stanie odróżnić go od kodu. Istnieje kilka sposobów, aby to zrobić.

- Należy wpisać tekst w elemencie HTML, takich jak `<p></p>` lub `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    HTML element może zawierać tekstu, dodatkowe elementy HTML i wyrażenia w kodzie serwera. Gdy program ASP.NET widzi otwierający HTML tag (na przykład `<p>`), powoduje wyświetlenie, wszystko w tym elemencie, a jej zawartość jako do przeglądarki, rozpoznawanie kodu serwera wyrażenia, ponieważ przechodzi ona.
- Użyj `@:` operatora lub `<text>` elementu. `@:` Generuje pojedynczy wiersz zawartość zawierająca zwykły tekst lub niedopasowane tagów HTML; `<text>` element zawiera wiele wierszy w danych wyjściowych. Te opcje są przydatne, jeśli nie chcesz renderować HTML element jako część danych wyjściowych.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    W danych wyjściowych wiele wierszy tekstu lub niedopasowane tagi HTML, można poprzedzić każdy wiersz z `@:`, lub może być częścią wiersza w `<text>` elementu. Podobnie jak `@:` operatora`<text>` znaczniki są używane przez program ASP.NET do identyfikowania zawartości tekstowej i nigdy nie są renderowane w danych wyjściowych strony.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Pierwszy przykład jest powtarzany w poprzednim przykładzie, ale używa jedna para `<text>` tagów, aby ująć tekstu do renderowania. W drugim przykładzie `<text>` i `</text>` tagi należy ująć w trzy wiersze, które mają niektóre niedopasowane tagi HTML i uncontained tekst (`<br />`), wraz z kodu serwera i dopasowane tagów HTML. Ponownie, również może poprzedzać każdy wiersz za pomocą `@:` operator; albo sposób działania.

    > [!NOTE]
    > Gdy tekstu wyjściowego przedstawione w tej sekcji &#8212; przy użyciu elementu HTML `@:` operatora lub `<text>` elementu &#8212; ASP.NET nie kodowanie HTML dane wyjściowe. (Jak wspomniano wcześniej, ASP.NET, kodowania danych wyjściowych wyrażeń kodu serwera i serwera bloków kodu, które są poprzedzone `@`, z wyjątkiem specjalnych przypadków wymienionych w tej sekcji.)

### <a name="whitespace"></a>Białe znaki

Dodatkowe spacje w instrukcji (i poza literału ciągu), nie mają wpływu na instrukcji:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Podział wiersza w instrukcji nie ma wpływu na instrukcji i może zawijać się instrukcje, aby zwiększyć czytelność. Poniższe instrukcje są takie same:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Jednak nie można opakować linię trakcie literału ciągu. Poniższy przykład nie działa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Aby połączyć długi ciąg, który jest zawijany do wielu linii, takich jak kod powyżej, dostępne są dwie opcje. Możesz użyć operatora łączenia (`+`), które zostaną wyświetlone w dalszej części tego artykułu. Można również użyć `@` znaku, aby utworzyć ciąg verbatim literału, jak przedstawiono wcześniej w tym artykule. Wielu liniach, można podzielić ciąg verbatim literałów:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Kod (i znaczników) komentarze

Komentarze umożliwiają pozostawienie notatki dla siebie i innych osób. Umożliwiają także wyłączyć (*komentarz*) części kodu lub języka znaczników, nie chcesz uruchamiać przez użytkownika, które mają być zachowane w na stronie przez pewien czas.

Istnieje inny, komentowanie składnię Razor kod oraz kod znaczników HTML. Podobnie jak w przypadku wszystkich kodu Razor, komentarz Razor są przetwarzane (i następnie usuwane) na serwerze przed wysłaniem strony do przeglądarki. W związku z tym ze składni Razor komentowania umożliwia umieść komentarze w kodzie (lub nawet do znaczników), widoczne podczas edycji pliku, ale użytkownicy nie widzą, nawet w przypadku źródła strony.

Komentarze ASP.NET Razor start komentarz z `@*` i Zakończ ją za pomocą `*@`. Komentarz może być w jednym lub wielu wierszy:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Poniżej przedstawiono komentarz w bloku kodu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

W tym miejscu jest tego samego bloku kodu, w wierszu kodu oznaczone jako komentarz, aby nie mogą być uruchamiane:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Wewnątrz bloku kodu zamiast przy użyciu składni komentarza Razor, można użyć komentowania składni języka programowania, którego używasz, takich jak C#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

W języku C# są poprzedzone Komentarze jednowierszowe `//` znaków i wielowierszowe komentarze zaczynają się od `/*` i kończyć się znakiem `*/`. (Podobnie jak w przypadku komentarz Razor komentarze języka C# nie są uważane za do przeglądarki.)

W przypadku znaczników prawdopodobnie wiesz, można utworzyć komentarz HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Komentarze HTML rozpoczynać `<!--` znaków oraz kończyć się `-->`. Można użyć komentarze HTML otoczyć nie tylko tekst, ale również wszelkich znacznik HTML, który może chcesz zachować na stronie, ale nie ma być renderowany. Ten komentarz HTML spowoduje ukrycie całą zawartość tagi i tekst, które zawierają:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

W przeciwieństwie do komentarzy Razor, HTML komentarze *są* renderowania do strony i użytkownik może je wyświetlić, wyświetlając źródła strony.

Razor ma ograniczenia zagnieżdżone bloki konstrukcyjne C#. Aby uzyskać więcej informacji, zobacz [o nazwie zmienne języka C# i zagnieżdżone bloki Generuj uszkodzony kod](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Zmienne

Zmienna jest nazwany obiekt, który służy do przechowywania danych. Zmienne możesz nazwać wszystko, ale nazwa musi rozpoczynać się od litery alfabetu i nie może zawierać spacji ani znaków zarezerwowanych.

### <a name="variables-and-data-types"></a>Zmienne i typy danych

Zmienna może mieć na określony typ danych, która wskazuje, jakiego typu dane są przechowywane w zmiennej. Mogą mieć zmiennych ciągu, które przechowują wartości ciągu (takich jak &quot;Witaj, świecie&quot;), zmiennych całkowitych, które przechowują wartości liczby całkowitej (np. 3 lub 79) i zmienne daty, które przechowywanie wartości daty w różnych formatach (np. 4/12/2012 lub marca 2009 ). Wiąże się wiele typów danych, w których można użyć.

Jednak zazwyczaj nie trzeba określać typu zmiennej. W większości przypadków, platformy ASP.NET można ustalić typu, w oparciu sposobu korzystania z danych w zmiennej. (Czasami musisz określić typ; pojawi się przykłady którym ta zasada obowiązuje).

Możesz deklarować zmienną za pomocą `var` — słowo kluczowe (Jeśli nie chcesz określić typ) lub za pomocą nazwy typu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Poniższy przykład przedstawia niektóre typowe zastosowania zmiennych na stronie sieci web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Jeśli łączone poprzednich przykładach, na stronie widzisz taki komunikat wyświetlany w przeglądarce:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konwertowanie i testowanie typów danych

Mimo że program ASP.NET zazwyczaj może automatycznie określić typu danych, czasami nie jest. W związku z tym konieczne może być pomógł ASP.NET, wykonując jawnej konwersji. Nawet jeśli nie masz konwersji typów, czasami warto sprawdzić, jakiego typu dane być może pracujesz z.

Najbardziej często zdarza się, trzeba przekonwertować ciąg do innego typu, takie jak liczbą całkowitą lub daty. Poniższy przykład przedstawia typową sytuacją, w którym należy przekonwertować ciąg na liczbę.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Zgodnie z zasadą dane wejściowe użytkownika dostarczany jako ciągi. Nawet jeśli po wyświetleniu monitu użytkowników podania numeru, a nawet wtedy, gdy zostały wprowadzone cyfr, podczas przesyłania danych wejściowych użytkownika, a następnie go odczytać w kodzie, dane są w formacie ciągu. W związku z tym należy przekonwertować ciąg na liczbę. W przykładzie Jeśli zostanie podjęta próba wykonania operacji arytmetycznych na wartościach, bez konwersji, następujący błąd wyników, ponieważ program ASP.NET nie można dodać dwa ciągi:

*Nie można niejawnie przekonwertować typu "string" do "int".*

Aby przekonwertować wartości liczb całkowitych, należy wywołać `AsInt` metody. Jeśli konwersja się pomyślnie, następnie można dodać liczb.

Poniższa lista zawiera niektóre typowe metody konwersji i testowania dla zmiennych.

::: wiersz:::::: kolumny::: <strong>metoda</strong> ::: koniec kolumny:::::: kolumny::: <strong>opis</strong> ::: koniec kolumny:::::: kolumny::: <strong>przykład</strong> ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsInt(), IsInt()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, który reprezentuje liczbę całkowitą z zakresu (np. "593") na liczbę całkowitą.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsBool(), IsBool()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, takich jak &quot;true&quot; lub &quot;false&quot; na typ Boolean.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsFloat(), IsFloat()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, który ma wartość dziesiętną, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę zmiennoprzecinkową.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsDecimal(), IsDecimal()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, który ma wartość dziesiętną, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę dziesiętną. (W programie ASP.NET: liczba dziesiętna jest bardziej precyzyjne niż liczba zmiennoprzecinkowa). ::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsDateTime(), IsDateTime()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg reprezentujący wartość daty i godziny ASP.NET `DateTime` typu.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `ToString()` ::: koniec kolumny:::::: kolumny::: jakimkolwiek innym typem danych jest konwertowany na ciąg.
::: koniec kolumny:::::: kolumny::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    ::: koniec kolumny:::::: końca wiersza:::

## <a name="operators"></a>Operatory

Operator jest słowo kluczowe lub znak, który informuje ASP.NET, jakiego rodzaju polecenie do wykonania w wyrażeniu. W języku C# (i składni Razor, który opiera się na nim) obsługuje wiele operatorów, ale musisz rozpoznać kilka, aby rozpocząć pracę. Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.


::: wiersz:::::: kolumny::: <strong>Operator</strong> ::: koniec kolumny:::::: kolumny::: <strong>opis</strong> ::: koniec kolumny:::::: kolumny::: <strong>przykłady</strong> ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `+` `-` `*` `/` ::: koniec kolumny:::::: kolumny::: operatorów matematycznych używać w wyrażeniach liczbowych.
::: koniec kolumny:::::: kolumny::: [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `=` ::: koniec kolumny:::::: kolumny::: przypisania. Przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `==` ::: koniec kolumny:::::: kolumny::: równości. Zwraca `true` Jeśli wartości są równe. (Należy zauważyć różnicę między `=` operatora i `==` operatora.)::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `!=` ::: koniec kolumny:::::: kolumny::: nierówności. Zwraca `true` wartości nie są równe.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `< > <= >=` ::: koniec kolumny:::::: kolumny::: mniej-niż, większe-niż, mniejsze niż lub równości i większa niż — lub równości.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `+` ::: koniec kolumny:::::: kolumny::: łączenia, który jest używany do łączenia ciągów. ASP.NET wie, że różnica między Ten operator i operator dodawania, na podstawie typu danych wyrażenia.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `+=` `-=` ::: koniec kolumny:::::: kolumny::: operatorów inkrementacji i dekrementacji, które dodawania i odejmowania 1 (odpowiednio) ze zmiennej.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `.` ::: koniec kolumny:::::: kolumny::: kropka. Używany do odróżnienia obiektów i ich właściwości i metody.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `()` ::: koniec kolumny:::::: kolumny::: nawiasów. Używane do wyrażenia grupy oraz w celu przekazania parametrów do metod.
::: koniec kolumny:::::: kolumny::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `[]` ::: koniec kolumny:::::: kolumny::: nawiasy kwadratowe. Używane do uzyskiwania dostępu do wartości w tablice i kolekcje.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `!` ::: koniec kolumny:::::: kolumny::: nie. Odwraca `true` wartość `false` i na odwrót. Zwykle jest używana jako sposób skrót do testowania `false` (oznacza to, aby nie `true`).
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `&&` <code>&#124;&#124;</code> ::: koniec kolumny:::::: kolumny::: logiczne i i lub które są używane do łączenia ze sobą warunki.
::: koniec kolumny:::::: kolumny::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    ::: koniec kolumny:::::: końca wiersza:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Praca z pliku i ścieżki folderu, w kodzie

Będziesz często pracować ze ścieżkami plików i folderów w kodzie. Oto przykład strukturę folderu fizycznego dla witryny sieci Web mogą być wyświetlane na komputerze deweloperskim:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Poniżej przedstawiono niektóre podstawowe szczegóły dotyczące adresów URL i ścieżki:

- Adres URL zaczyna się od jedną nazwę domeny (`http://www.example.com`) lub nazwy serwera (`http://localhost`, `http://mycomputer`).
- Adres URL odnosi się do ścieżki fizycznej na komputerze-hoście. Na przykład `http://myserver` może odpowiadać folder *C:\websites\mywebsite* na serwerze.
- Ścieżka wirtualna jest skrótem do reprezentowania ścieżek w kodzie bez konieczności podać pełną ścieżkę. Obejmuje on część adresu URL, który następuje nazwa domeny lub serwera. Gdy używasz ścieżki wirtualne, kod można przenieść do innej domeny lub serwer, bez konieczności aktualizowania ścieżki.

Oto przykład, aby lepiej zrozumieć różnice:

| Pełny adres URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nazwa serwera | *mycompanyserver* |
| Ścieżka wirtualna | */humanresources/CompanyPolicy.htm* |
| Ścieżka fizyczna | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Wirtualny katalog główny jest /, podobnie jak głównego C: dysku \. (Folder wirtualny ścieżki zawsze używać ukośników.) Ścieżka wirtualna folderu nie musi mieć taką samą nazwę jak folder fizycznych; może być aliasem. (Na serwerach produkcyjnych, ścieżka wirtualna rzadko odpowiada dokładnej ścieżki fizycznej.)

Podczas pracy z plikami i folderami w kodzie, czasami musisz odwoływać się do ścieżki fizycznej, a czasami ścieżką wirtualną, w zależności od tego, jakie obiekty pracujemy z użyciem. ASP.NET udostępnia te narzędzia do pracy ze ścieżkami plików i folderów w kodzie: `Server.MapPath` metody i `~` operatora i `Href` metody.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Konwertowanie ścieżki wirtualnej do fizycznego: Metoda Server.MapPath

`Server.MapPath` Metoda konwertuje ścieżki wirtualnej (np. */default.cshtml*) do ścieżki bezwzględnej fizycznych (takich jak *C:\WebSites\MyWebSiteFolder\default.cshtml*). Możesz użyć tej metody w każdej chwili należy pełną ścieżkę fizyczną. Typowym przykładem jest podczas odczytywania lub zapisywania pliku tekstowego lub pliku obrazu na serwerze sieci web.

Zazwyczaj nie wiesz, bezwzględną ścieżkę fizyczną witryny na serwerze lokacji hostingu, więc tej metody można przekonwertować ścieżkę wiadomo — ścieżka wirtualna — do odpowiedniej ścieżki na serwerze dla Ciebie. Ścieżka wirtualna jest przekazywane do pliku lub folderu, do metody i zwraca ścieżkę fizyczną:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odwoływanie się do wirtualnego katalogu głównego: ~ operatora i Href, metoda

W *.cshtml* lub *.vbhtml* pliku, możesz odwoływać się za pomocą ścieżka wirtualnego katalogu głównego `~` operatora. Może to być bardzo przydatne, ponieważ możesz poruszać się strony w witrynie, a wszystkie łącza, które zawierają do innych stron nie będzie działać. Jest to również przydatne w przypadku, gdy przeniesiesz kiedykolwiek witryny sieci Web w innej lokalizacji. Oto kilka przykładów:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Jeśli witryna sieci Web `http://myserver/myapp`, Oto jak ASP.NET będzie traktować te ścieżki, po uruchomieniu na stronie:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Te ścieżki faktycznie nie będzie widoczna jako wartości zmiennej, ale ASP.NET traktują ścieżki, tak, jakby to, jakie były).

Możesz użyć `~` operator zarówno w kodzie serwera (jak powyżej), jak i znaczników w następujący sposób:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

W znaczniku, użyj `~` operatora do utworzenia ścieżki do zasobów, takich jak pliki obrazów, innych stron sieci web i plików CSS. Po uruchomieniu strony ASP.NET przegląda strony (kodu i znaczników) i usuwa wszystkie `~` odwołania do danej ścieżce.

## <a name="conditional-logic-and-loops"></a>Logika warunkowe i pętle

Kod serwera ASP.NET umożliwia wykonywanie zadań na podstawie warunków i napisać kod, który powtarza instrukcji określoną liczbę razy (czyli kodu wykonywanego w pętli).

### <a name="testing-conditions"></a>Warunki badania

Aby przetestować warunku prostego, możesz użyć `if` instrukcję, która zwraca wartość PRAWDA lub FAŁSZ, oparte na test, należy określić:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if` — Słowo kluczowe uruchamia blok. Test rzeczywisty (stan) w nawiasach i zwraca wartość PRAWDA lub FAŁSZ. Instrukcje, które uruchomienia, jeśli test jest wartość true, są ujęte w nawiasy klamrowe. `if` Może zawierać instrukcji `else` blok, który określa instrukcje do uruchomienia, jeśli warunek nie jest spełniony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Można dodać wiele warunków przy użyciu `else if` bloku:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

W tym przykładzie, jeśli pierwszy warunek w instrukcji if bloku nie ma wartość true, `else if` warunek jest sprawdzany. Jeśli ten warunek jest spełniony, instrukcje w `else if` bloku są wykonywane. Jeśli żaden z warunków nie jest spełniony, instrukcje w `else` bloku są wykonywane. Można dodać dowolną liczbę else if blokuje, a następnie Zamknij z `else` zablokować, jako &quot;cała reszta&quot; warunku.

Aby przetestować dużą liczbę warunków, należy użyć `switch` bloku:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Wartość do sprawdzenia jest w nawiasach (w tym przykładzie `weekday` zmiennej). Każdy pojedynczy test używa `case` instrukcję, która kończy się wraz z dwukropkiem (:). Jeśli wartość `case` instrukcji pasuje do wartości testu, wykonywany jest kod, w tym przypadku bloku. Zamknij każdej instrukcji case z `break` instrukcji. (Jeśli zapomnisz o wstawieniu podziału w każdym `case` bloku kodu z następnej `case` instrukcji zostaną również uruchomione.) A `switch` blok często ma `default` instrukcję jako ostatni przypadku &quot;cała reszta&quot; opcja, która jest uruchamiana, gdy żaden z pozostałych przypadkach jest spełniony.

Wynik ostatnich dwóch bloków warunkowych, wyświetlany w przeglądarce:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Tworzenie pętli kodu

Często muszą uruchomić te same instrukcje wielokrotnie. W tym pętli są uwzględnione. Na przykład można często uruchamiane te same instrukcje dla każdego elementu w kolekcji danych. Jeśli wiesz, dokładnie tak jak często chcesz pętli, możesz użyć `for` pętli. Ten rodzaj pętli jest szczególnie przydatna podczas liczenia w lub inwentaryzacji:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Pętla zaczyna się od `for` — słowo kluczowe następują trzy instrukcje w nawiasach, każdy kończyć się średnikiem.

- Wewnątrz nawiasów w pierwszej instrukcji (`var i=10;`) tworzy licznika i inicjuje ją do 10. Nie masz nazwy licznika `i` &#8212; można użyć dowolnej zmiennej. Gdy `for` kiedy pętla zostaje uruchomiona, licznik jest automatycznie zwiększany.
- Druga instrukcja (`i < 21;`) określa warunek, o ile liczba. W tym przypadku chcesz przejść do maksymalnie 20 (czyli kontynuować pracę podczas gdy ten licznik jest mniej niż 21).
- Trzecia instrukcja (`i++` ) używa operator inkrementacji, który po prostu Określa, że licznik powinny mieć 1 dodawanych do niego przy każdym uruchomieniu pętli.

Wewnątrz nawiasów klamrowych jest kod, który będzie wykonywany dla każdej iteracji pętli. Znaczniki, tworzy nowy akapit (`<p>` elementu) każdego czas i dodaje wiersz danych wyjściowych, wyświetlanie wartości `i` (licznik). Po uruchomieniu tej strony, ten przykład tworzy 11 wiersza, wyświetlanie danych wyjściowych z tekstem w każdym wierszu, wskazującą liczbę elementów.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Jeśli pracujesz z kolekcji lub tablicy, są często używane `foreach` pętli. Kolekcja to grupa podobnych obiektów i `foreach` pętli pozwala przeprowadzić zadanie dla każdego elementu w kolekcji. Ten typ pętli jest wygodne w przypadku kolekcji, ponieważ w odróżnieniu od `for` pętli, nie trzeba ten licznik lub Ustaw limit. Zamiast tego `foreach` kodu pętli po prostu obejmującego kolekcji dopiero po jej zakończeniu.

Na przykład, poniższy kod zwraca elementy w `Request.ServerVariables` kolekcji, która jest obiektem, który zawiera informacje o serwerze sieci web. Używa ona `foreac` h pętli, aby wyświetlić nazwę każdego elementu przez utworzenie nowego `<li>` element na liście punktowanej we wcześniejszej HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach` — Słowo kluczowe, następuje nawiasy, o których należy zadeklarować zmienną, która reprezentuje pojedynczy element w kolekcji (w tym przykładzie `var item`), a następnie `in` — słowo kluczowe, następuje kolekcję ma pętli. W treści `foreach` pętli, możesz uzyskać dostęp bieżącego elementu przy użyciu zmiennej zadeklarowanej wcześniej.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Aby utworzyć bardziej ogólnego przeznaczenia pętli, użyj `while` instrukcji:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A `while` pętli zaczyna się od `while` — słowo kluczowe, następuje nawiasów której określić, jak długo będzie nadal pętli (tutaj, aby uzyskać tak długo, jak `countNum` jest mniejsza niż 50), następnie bloku powtórzeń. Zazwyczaj Zwiększ pętli (dodać) lub dekrementacji (odjęcia od) zmiennej lub obiektu użytego do zliczania. W tym przykładzie `+=` operator dodaje 1- `countNum` przy każdym uruchomieniu pętli. (W celu dekrementacja zmiennej w pętlę, który odlicza, należy użyć operatora dekrementacyjnego `-=`).

## <a name="objects-and-collections"></a>Obiekty i kolekcje

Prawie wszystko w witrynie internetowej platformy ASP.NET jest obiektem, w tym strona sieci web. W tej sekcji omówiono niektóre ważne obiekty, które często będziesz pracować z w kodzie.

### <a name="page-objects"></a>Obiekty strony

Najbardziej podstawowa obiektu w programie ASP.NET: jest to strona. Możesz uzyskać dostęp właściwości obiektu strony bezpośrednio, bez dowolnych obiektów kwalifikujących. Poniższy kod pobiera ścieżkę do pliku, przy użyciu `Request` obiekt strony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Aby stał się jasne, możesz teraz odwołuje się do właściwości i metody dla bieżącego obiektu strony możesz opcjonalnie użyć słowa kluczowego `this` obiekt strony w kodzie. Poniżej przedstawiono w poprzednim przykładzie kodu za pomocą `this` dodane do reprezentowania strony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Można użyć właściwości `Page` można uzyskać mnóstwo informacji, takich jak:

- `Request`. Jak już przeczytane, to zbiór informacji o bieżącym żądaniu, w tym, jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp.
- `Response`. Jest to zbiór informacji na temat odpowiedzi (strona), który będzie wysyłany do przeglądarki, gdy kod serwera zakończył działanie. Na przykład można używasz tej właściwości można zapisać informacji do odpowiedzi. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Obiekty kolekcji (tablic i słowników)

A *kolekcji* jest grupą obiektów tego samego typu, na przykład zbiór `Customer` obiektów z bazy danych. Program ASP.NET zawiera wiele kolekcji wbudowanych, takie jak `Request.Files` kolekcji.

Często będziesz pracować z danymi w kolekcji. Istnieją dwa typy kolekcji typowe *tablicy* i *słownika*. Tablica jest przydatne, gdy ma być przechowywany w kolekcji podobnych elementów, ale nie chcesz utworzyć osobne zmienną do przechowywania każdego elementu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

W przypadku tablic, możesz zadeklarować na określony typ danych, takich jak `string`, `int`, lub `DateTime`. Aby wskazać, że zmienna może zawierać tablicę, Dodaj nawiasów do deklaracji (takie jak `string[]` lub `int[]`). Możesz uzyskać dostęp elementów w tablicy przy użyciu ich pozycji (indeks) lub za pomocą `foreach` instrukcji. Indeksy tablicy są oparte na zerze &#8212; oznacza to, pierwszy element jest na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Można określić liczbę elementów w tablicy, uzyskując jego `Length` właściwości. Aby uzyskać położenie konkretny element w tablicy (na przykład aby wyszukać), użyj `Array.IndexOf` metody. Można również wykonać elementów, takich jak wstecznego zawartość tablicy ( `Array.Reverse` metoda) i sortować zawartość ( `Array.Sort` metody).

Dane wyjściowe kodu tablicy ciąg wyświetlany w przeglądarce:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Słownik to kolekcja par klucz wartość, określane klucz (lub nazwę) można ustawić lub pobrać odpowiednią wartość:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Aby utworzyć słownik, należy użyć `new` — słowo kluczowe, aby wskazać, czy tworzysz nowy obiekt słownika. Słownik można przypisać do zmiennej za pomocą `var` — słowo kluczowe. Możesz określać typy danych elementów w słowniku przy użyciu nawiasów ( `< >` ). Na końcu deklaracji należy dodać parę nawiasów, ponieważ jest to rzeczywiście metodę, która tworzy nowy słownik.

Aby dodać elementy do słownika, możesz wywołać `Add` metody na zmienną słownika (`myScores` w tym przypadku), a następnie określ klucz i wartość. Alternatywnie można nawiasami kwadratowymi klucza, a następnie wykonaj przypisanie proste, jak w poniższym przykładzie:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Wywołanie metody z parametrami

Podczas czytania we wcześniejszej części tego artykułu, obiekty, które program za pomocą może mieć metody. Na przykład `Database` obiekt może mieć `Database.Connect` metody. Wiele metod również mieć co najmniej jeden parametr. A *parametru* jest wartością, który jest przekazywany do metody, aby włączyć metodę w celu zakończenia zadania. Na przykład Przyjrzyj się deklarację dla `Request.MapPath` metody, która przyjmuje trzy parametry:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Wiersz został opakowany w celu poprawienia czytelności. Należy pamiętać, umieścić podziały wierszy niemal dowolnym miejscu, z wyjątkiem wewnątrz ciągów, które są ujęte w znaki cudzysłowu.)

Ta metoda zwraca ścieżkę fizyczną na serwerze, który odnosi się do określonej ścieżki wirtualnej. Są trzy parametry dla metody `virtualPath`, `baseVirtualDir`, i `allowCrossAppMapping`. (Zwróć uwagę, że w deklaracji, parametry są wyświetlane z typami danych, który będzie mógł zaakceptować). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.

Składnia Razor daje dwie opcje do przekazywania parametrów do metody: *parametry pozycyjne* i *nazwanych parametrów*. Aby wywołać metodę, przy użyciu parametry pozycyjne, parametry są przekazywane w kolejności strict, który jest określony w deklaracji metody. (Będzie zazwyczaj wiesz to zamówienie, przeczytaj dokumentację dla metody.) Należy wykonać, kolejność i nie można pominąć dowolny z parametrów &#8212; Jeśli to konieczne, możesz przekazać pusty ciąg (`""`) lub `null` parametru pozycyjnych, które nie mają wartości.

W poniższym przykładzie założono, masz folder o nazwie *skrypty* w witrynie sieci Web. Kod wywołuje `Request.MapPath` metody i przekazuje wartości dla trzech parametrów w odpowiedniej kolejności. Następnie wyświetla ścieżki wynikowej zamapowany.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Gdy metoda ma wiele parametrów, możesz przechowywać swój kod bardziej czytelny przy użyciu nazwanych parametrów. Aby wywołać metodę, przy użyciu nazwanych parametrów, należy określić nazwę parametru, po której następuje dwukropek (:), a następnie wartość. Zaletą nazwanych parametrów jest to, że można przekazać je w dowolnej kolejności, które mają. (Wadą jest wywołanie metody nie jest najmniejszy).

Poniższy przykład wywołuje tę samą metodę jak powyżej, ale korzysta z nazwanych parametrów, aby podać wartości:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Jak widać, parametry są przekazywane w innej kolejności. Jednak po uruchomieniu poprzedniego przykładu i w tym przykładzie zostanie zwrócona taką samą wartość.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Obsługa błędów

### <a name="try-catch-statements"></a>Instrukcje try-Catch

Często masz instrukcji w kodzie, który może zakończyć się niepowodzeniem ze względów poza Twoją kontrolą. Na przykład:

- Jeśli kod próbuje utworzyć lub uzyskać dostęp do pliku, mogą wystąpić wszelkiego rodzaju błędów. Żądany plik nie istnieje, może być zablokowana, kod może nie mieć uprawnień i tak dalej.
- Podobnie jeśli kod próbuje zaktualizować rekordy w bazie danych, może to być problemy z uprawnieniami, mogą być opuszczane połączenia z bazą danych, dane w celu zaoszczędzenia może być nieprawidłowy i tak dalej.

W sposób programowania, są nazywane tych sytuacji *wyjątki*. Jeśli Twój kod napotka wyjątek, generuje (zgłasza) komunikat o błędzie przykład w najlepszym irytujących dla użytkowników:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

W sytuacjach, w którym kod może wystąpić wyjątki i w celu uniknięcia komunikaty o błędach tego typu można użyć `try/catch` instrukcji. W `try` instrukcji, uruchom kod, który jest sprawdzany. W co najmniej jednej `catch` instrukcji, można wyszukać konkretnego błędów (określonych typów wyjątków), które mogły wystąpić. Może zawierać tyle `catch` instrukcje, jak należy sprawdzić występowanie błędów, które są przewidywania.

> [!NOTE]
> Firma Microsoft zaleca, unikaj używania `Response.Redirect` method in Class metoda `try/catch` instrukcji, ponieważ może to spowodować wyjątek na stronie.


Poniższy przykład pokazuje stronę, która tworzy plik tekstowy na pierwsze żądanie, a następnie wyświetla przycisk, który umożliwia użytkownikowi, otwórz plik. W przykładzie użyto celowo Zła nazwa pliku, tak aby spowoduje wyjątek. Kod zawiera `catch` instrukcji dla dwóch możliwych wyjątków: `FileNotFoundException`, który występuje, jeśli nazwa pliku jest nieprawidłowa, i `DirectoryNotFoundException`, który występuje, gdy program ASP.NET nawet nie można odnaleźć folderu. (Aby zobaczyć, jak działa, gdy wszystko będzie działać prawidłowo można komentarz do instrukcji w tym przykładzie).

Jeśli Twój kod nie obsługuje wyjątek, widział takich jak poprzedniej zrzut ekranu przedstawiający stronę błędu. Jednak `try/catch` sekcji uniemożliwia użytkownikowi widzisz błędy tych typów.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

**Programowanie za pomocą języka Visual Basic**


[Dodatku: język Visual Basic i składnię](https://go.microsoft.com/fwlink/?LinkId=202908)


**Dokumentacja referencyjna**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[W języku C#](https://msdn.microsoft.com/library/kx37x362.aspx)
