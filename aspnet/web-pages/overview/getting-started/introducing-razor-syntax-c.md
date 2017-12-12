---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: "Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor (C#) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym rozdziale zapewnia przegląd programowania ze strony sieci Web ASP.NET przy użyciu składni Razor. ASP.NET to technologia firmy Microsoft dotyczące uruchamiania dynamicznych sieci Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: f054d574026ab6444cc59a126ef9dcdc323f7bff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor (C#)
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule zapewnia przegląd programowania ze strony sieci Web ASP.NET przy użyciu składni Razor. Program ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznych stron sieci web na serwerach sieci web. Ten artykuł zespoły przy użyciu języka programowania w języku C#.
> 
> **Dowiesz się**:
> 
> - 8 top, programowania porady dotyczące wprowadzenie do programowania przy użyciu składni Razor strony sieci Web ASP.NET.
> - Podstawowe pojęcia programowania, które będą potrzebne.
> - Jakie kodu serwera ASP.NET o składni Razor są wszystkie informacje.
>   
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Górny 8 porady dotyczące programowania

W tej sekcji przedstawiono kilka wskazówek, które należy bezwzględnie znać rozpoczęcie pisania kodu serwera ASP.NET przy użyciu składni Razor.

> [!NOTE]
> Składnia Razor jest oparta na język programowania C#, a to język, który jest najczęściej używany z ASP.NET Web Pages. Jednak ze składni Razor obsługuje również języka Visual Basic i wszystkie obiekty, które można zobaczyć, że możesz również wykonać w języku Visual Basic. Aby uzyskać więcej informacji, zobacz dodatek [języka Visual Basic i składni](https://go.microsoft.com/fwlink/?LinkId=202908).


W dalszej części tego artykułu można znaleźć więcej szczegółów na temat większości technik programowania.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Dodaj kod do strony przy użyciu znaku @

`@` Znak uruchamia wyrażenia wbudowany, jednej instrukcji bloki, a wielu instrukcji:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Jest to, jak te instrukcje wyglądać po uruchomieniu na stronie w przeglądarce:

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Kodowanie HTML**
> 
> Podczas wyświetlania zawartości na stronie za pomocą `@` znak w powyższych przykładach ASP.NET HTML-koduje dane wyjściowe. Spowoduje to zastąpienie zarezerwowanych znaków HTML (takich jak `<` i `>` i `&`) z kodami umożliwiających znaków, które mają być wyświetlane jako znaki na stronie sieci web, a nie interpretowany jako tagów HTML lub jednostek. Zakodowany w formacie HTML, dane wyjściowe w kodzie serwera może nie wyświetlać się poprawnie, a może spowodować narażenie strony na zagrożenia bezpieczeństwa.
> 
> Celem użytkownika jest output kod znaczników HTML, który renderuje tagi jako kod znaczników (na przykład `<p></p>` akapitu lub `<em></em>` aby podkreślić tekst), zobacz sekcję [łączenie tekstu, znaczników i kodu w bloki kodu](#BM_CombiningTextMarkupAndCode) dalszej części tego artykułu.
> 
> Możesz przeczytać więcej na temat kodowania HTML w [pracy z formularzami](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Umieść bloki kodu w nawiasach klamrowych

A *blok kodu* zawiera jedną lub więcej instrukcji kodu i jest ujęta w nawiasy klamrowe.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Wynik wyświetlony w przeglądarce:

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. W bloku kończyć każda instrukcja kodu średnikami

W bloku kodu każda instrukcja kompletny kod musi być zakończona średnikiem. Wbudowane wyrażenia nie kończyć się średnikiem.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. W przypadku używania zmiennych do przechowywania wartości

Można zapisać wartości w *zmiennej*, w tym ciągów, liczb i daty, itp. Można utworzyć nowej zmiennej przy użyciu `var` — słowo kluczowe. Można wstawić wartości zmiennych bezpośrednio za pomocą strony `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Wynik wyświetlony w przeglądarce:

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. To ująć w podwójny cudzysłów wartości literału ciągu

A *ciąg* jest sekwencji znaków, które są traktowane jako tekst. Aby określić ciąg, ująć w podwójny cudzysłów:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Jeśli ciąg, który chcesz wyświetlić zawiera znak ukośnika odwrotnego ( `\` ) lub podwójny cudzysłów ( `"` ), użyj *literał ciągu dosłownego wyrażenia* który prefiksem `@` operatora. (W języku C#, \ znak ma specjalnego znaczenia, chyba że za pomocą literału ciągu dosłownego wyrażenia.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Aby osadzić znaki cudzysłowu, użyj literał ciągu dosłownego wyrażenia i powtórz znaki cudzysłowu:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

W tym miejscu jest wynikiem za pomocą obu tych przykładach na stronie:

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Zwróć uwagę, że `@` znak jest używany do oznaczyć literał ciągu dosłownego wyrażenia w C# i oznacz kodu w stron ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Kod jest rozróżniana wielkość liter

W języku C#, słowa kluczowe (takich jak `var`, `true`, i `if`) i nazwy zmiennych jest uwzględniana wielkość liter. Następujące wiersze kodu utworzyć dwie różne zmienne, `lastName` i`LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Deklarowanie zmiennej jako `var lastName = "Smith";` i podjęcie próby odwołanie do tej zmiennej na stronie jako `@LastName`, powoduje błąd, ponieważ `LastName` nie zostanie rozpoznane.

> [!NOTE]
> W języku Visual Basic, słowa kluczowe i zmienne są *nie* z uwzględnieniem wielkości liter.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Obejmuje większość kodowania obiektów

*Obiektu* reprezentuje element, który zostanie z &#8212; strony, pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta (wiersza bazy danych), itp. Obiekty mają właściwości, które opisują ich właściwości i że może odczytać lub Zmień &#8212; Obiekt pola tekstowego ma `Text` właściwość (między innymi), obiekt żądania ma `Url` właściwość, wiadomości e-mail ma `From` właściwości oraz obiektu klienta ma `FirstName` właściwości. Obiekty mają również metody, które są &quot;zleceń&quot; mogą wykonywać. Przykładami obiektu pliku `Save` metodę, obiekt obrazu `Rotate` — metoda i obiektu poczty e-mail `Send` metody.

Często będzie współpracować `Request` obiektów, które zapewnia informacje, takie jak wartości pól tekstowych (pola formularza) na stronie, jakiego rodzaju przeglądarki zgłosił żądanie, adres URL strony tożsamości użytkownika, itp. Poniższy przykład przedstawia sposób uzyskać dostępu do właściwości `Request` obiektów i wywoływania `MapPath` metody `Request` obiektu, który daje użytkownikowi bezwzględna ścieżka strony na serwerze:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Wynik wyświetlony w przeglądarce:

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Można napisać kod, który podejmowania decyzji w procesie

Kluczową funkcją dynamicznej stron sieci web jest, że można określić, co zrobić, na podstawie warunków. Najczęściej w tym celu jest z `if` instrukcji (i opcjonalnie `else` instrukcji).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Instrukcja `if(IsPost)` jest skrócona sposobem zapisu `if(IsPost == true)`. Wraz z programem `if` instrukcji, istnieje wiele sposobów, aby przetestować warunki, powtórz bloki kodu, i itd., które są opisane w dalszej części tego artykułu.

Wynik wyświetlony w przeglądarce (po kliknięciu przycisku **przesyłania**):

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET i POST metod i właściwości IsPost
> 
> Protokół używany do stron sieci web (HTTP) obsługuje bardzo ograniczoną liczbę metod (zleceń), które są używane na wysyłanie żądań do serwera. Dwa najbardziej typowe to GET, który jest używany do odczytu stronę, i POST, który jest używany do przesyłania strony. Ogólnie rzecz biorąc po raz pierwszy użytkownik zażąda strony, strony jest wymagany przy użyciu GET. Jeśli użytkownik wypełnia formularza, a następnie klika przycisk Prześlij, przeglądarka wysyła żądanie POST do serwera.
> 
> W programowanie dla sieci web często jest grupowaniu można sprawdzić, czy strona jest wymagany jako GET lub POST, aby wiedzieć, jak przetwarzania tej strony. W składniku ASP.NET Web Pages można użyć `IsPost` właściwości, aby zobaczyć, czy żądanie jest GET lub POST. Jeśli żądanie jest żądaniem POST `IsPost` właściwości zwróci wartość true, a użytkownik może wykonywać następujące czynności odczytu wartości pól tekstowych w formularzu. Wiele przykładów zobaczysz opisano sposób przetwarzania strony inaczej w zależności od wartości `IsPost`.


## <a name="a-simple-code-example"></a>Prosty przykład kodu

Tej procedury przedstawiono sposób tworzenia strony, która ilustruje podstawowych technik programowania. W tym przykładzie utworzysz strony, który pozwala użytkownikom na wprowadzanie dwóch liczb, a następnie dodanie ich i wyświetla wyniki.

1. W edytorze, Utwórz nowy plik i nadaj jej nazwę *AddNumbers.cshtml*.
2. Skopiuj następujący kod i znaczników do strony, zastępując wszystko już na stronie.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Poniżej przedstawiono niektóre czynności umożliwiające należy zwrócić uwagę:

    - `@` Znak rozpoczyna się pierwszego bloku kodu na stronie i poprzedza `totalMessage` zmiennej, która jest osadzony w dolnej części strony.
    - Blok w górnej części strony jest ujęta w nawiasy klamrowe.
    - W bloku u góry wszystkie linie kończyć średnikiem.
    - Zmienne `total`, `num1`, `num2`, i `totalMessage` przechowywać wiele numerów i ciąg.
    - Wartość literału ciągu przypisane do `totalMessage` zmienna jest w podwójny cudzysłów.
    - Ponieważ kod jest rozróżniana wielkość liter, gdy `totalMessage` zmienna jest używana w dolnej części strony, jego nazwa musi być zgodny zmiennej u góry.
    - Wyrażenie `num1.AsInt() + num2.AsInt()` pokazano, jak pracować z obiektów i metod. `AsInt` Metody dla każdej zmiennej konwertuje ciąg wprowadzony przez użytkownika na liczbę (liczba całkowita) tak, aby można było wykonać operacje arytmetyczne na nim.
    - `<form>` Zawiera tag `method="post"` atrybutu. Oznacza to, że gdy użytkownik kliknie **Dodaj**, strony zostaną wysłane do serwera przy użyciu metody POST protokołu HTTP. Po przesłaniu strony `if(IsPost)` testu daje w wyniku wartość true i warunkowe kod działa wyświetlania wyniku dodawania liczb.
3. Strony i uruchom go w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.) Wprowadź dwie liczb całkowitych, a następnie kliknij przycisk **Dodaj** przycisku. 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Podstawowe koncepcje programowania

Ten artykuł zawiera omówienie programowania w języku sieci web ASP.NET. Nie jest kompletną badania, po prostu krótki przewodnik za pośrednictwem pojęcia dotyczące programowania, które będą używane najczęściej. Mimo tego obejmują prawie wszystko, które będą potrzebne, aby zacząć korzystać z ASP.NET Web Pages.

Ale pierwszy, małego informacje techniczne.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Składnia Razor kodu serwera i platformy ASP.NET

Składnia razor jest proste programowania składnię osadzania kodu na serwerze, na stronie sieci web. Na stronie sieci web, która używa składni Razor, istnieją dwa rodzaje zawartości: Kod klienta zawartości i serwera. Zawartość klienta jest rzeczy użyto do na stronach sieci web: kod znaczników HTML (elementy), informacje, takie jak CSS, styl może być niektórych skryptu klienta, takich jak JavaScript i zwykły tekst.

Składnia razor pozwala dodać kod serwera do tej zawartości klienta. Jeśli na stronie jest kod serwera, na serwerze działa ten kod najpierw przed wysłaniem strony do przeglądarki. Uruchamiając na serwerze, kod mogą wykonywać zadania, które mogą być znacznie bardziej złożone, aby zrobić przy użyciu klienta zawartość samodzielnie, takich jak uzyskiwanie dostępu na serwerze baz danych. Przede wszystkim kod serwera można dynamicznie utworzyć klienta, zawartość &#8212; go wygenerować kod znaczników HTML lub innej zawartości w locie, a następnie wyślij ją w przeglądarce wraz z statycznych kodu HTML, który może zawierać strony. Z perspektywy przeglądarki nie różni się od innej zawartości klient się zawartość klienta, która jest generowana przez kod serwera. Jak już przeczytane, kod serwera, która jest wymagana jest bardzo proste.

Strony sieci web ASP.NET zawierających składnię Razor mają rozszerzenie pliku specjalne (*.cshtml* lub *.vbhtml*). Serwer rozpoznaje tych rozszerzeń, uruchamia kod, który jest oznaczony atrybutem składni Razor, a następnie wysyła strony w przeglądarce.

### <a name="where-does-aspnet-fit-in"></a>Gdy program ASP.NET pasują do?

Składnia razor jest oparta na technologii firmy Microsoft o nazwie platformę ASP.NET, która z kolei jest oparta na programie Microsoft .NET Framework. Środowiska.NET Framework jest duży, kompleksowe struktury programistycznej firmy Microsoft związane z opracowywaniem praktycznie dowolnego typu aplikacji komputera. Program ASP.NET jest częścią programu .NET Framework, przeznaczone do tworzenia aplikacji sieci web. Deweloperzy użyto ASP.NET można utworzyć wiele witryn sieci Web z najwyższą ruchu i największa na świecie. (Dowolnej chwili wyświetlić rozszerzenie nazwy pliku *.aspx* jako część adresu URL w witrynie, będzie wiadomo, czy lokacji została opracowana za pomocą programu ASP.NET.)

Składnia Razor umożliwia wszystkie możliwości platformy ASP.NET, ale przy użyciu składni uproszczoną ułatwiający dowiedzieć się użytkownik początkujący i dzięki temu można efektywniej, jeśli jesteś eksperta. Mimo że ta składnia jest łatwy w użyciu, jej rodziny relacji z platformy ASP.NET i .NET Framework oznacza, że jako witryny sieci Web staną się bardziej zaawansowanych, być zasilania większych struktur, dostępne dla Ciebie.

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Klasy i wystąpienia**
> 
> Obiekty, które z kolei są tworzone z myślą klasy korzysta z kodu serwera ASP.NET. Klasa jest definicja lub szablon dla obiekt. Na przykład aplikacja może zawierać `Customer` klasa, która definiuje właściwości i metody, które wymaga dowolnego obiektu klienta.
> 
> Kiedy aplikacja musi działać z informacjami o klientach rzeczywiste, tworzy wystąpienia (lub *tworzy*) obiektu klienta. Poszczególnych klientów jest oddzielnego wystąpienia `Customer` klasy. Każde wystąpienie obsługuje te same właściwości i metody, ale wartości właściwości dla każdego wystąpienia różnią się zwykle, ponieważ każdy obiekt klienta jest unikatowa. W obiekcie jednego klienta `LastName` właściwość może być "Smith"; w innym obiekcie klienta, `LastName` właściwość może być "Nowak".
> 
> Podobnie jest indywidualnych stronach sieci web w swojej witrynie `Page` obiekt, który jest wystąpieniem `Page` klasy. Przycisk na stronie jest `Button` obiekt, który jest wystąpieniem `Button` klasy i tak dalej. Każde wystąpienie ma własne charakterystyki, ale wszystkie one są oparte na to, co jest określona w definicji klasy obiektu.


## <a name="basic-syntax"></a>Podstawowa składnia

Widać wcześniej podstawowy przykład sposobu tworzenia strony ASP.NET Web Pages i jak można dodać kod serwera do kod znaczników HTML. Tutaj dowiesz się podstawowe informacje dotyczące pisania kodu serwera ASP.NET przy użyciu składni Razor &#8212; oznacza to, że programowania reguł języka.

Jeśli masz doświadczenia w pracy z programowania (zwłaszcza, jeśli używano C, C++, C#, Visual Basic lub JavaScript), większość tutaj odczytu jest znane. Prawdopodobnie należy zapoznać się z tylko sposób kod serwera jest dodawana do kodu znaczników w *.cshtml* plików.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Łączenie tekstu, znaczników i kodu w bloki kodu

W serwerze bloki kodu ma często dane wyjściowe tekstu lub znacznika (lub obie) do strony. Jeśli blok kodu serwera zawiera tekst, który nie jest kodem i który zamiast tego ma być renderowany jako jest, ASP.NET musi mieć możliwość rozróżnienia ten tekst z kodu. Istnieje kilka sposobów, aby to zrobić.

- Tekst należy ująć w elemencie HTML, takie jak `<p></p>` lub `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    HTML element mogą zawierać tekst, dodatkowe elementy HTML i wyrażenia kod serwera. Gdy ASP.NET widzi otwierający HTML tag (na przykład `<p>`), wszystkie elementy w tym elemencie renderowania i zawartość jako jest w przeglądarce, rozpoznawania kod serwera wyrażenia, ponieważ przechodzi ona.
- Użyj `@:` operator lub `<text>` elementu. `@:` Generuje pojedynczy wiersz zawartości zawierającej zwykłego tekstu lub niedopasowane znaczniki HTML; `<text>` elementu obejmuje wiele wierszy danych wyjściowych. Te opcje są przydatne, jeśli nie chcesz do renderowania elementu HTML jako część danych wyjściowych.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Chcesz output wiele wierszy tekstu lub niedopasowane znaczniki HTML, mogą występować przed każdej linii `@:`, lub może być częścią linię w `<text>` elementu. Podobnie jak `@:` operatora`<text>` tagi są używane przez program ASP.NET do identyfikowania zawartości tekstowej i nigdy nie są renderowane w danych wyjściowych strony.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Pierwszym przykładzie powtarza poprzednim przykładzie, ale używa jednej pary `<text>` znaczniki, aby umieścić tekst do renderowania. W drugim przykładzie `<text>` i `</text>` tagi należy ująć w trzy wiersze, które mają niektóre uncontained tekst i niedopasowane znaczniki HTML (`<br />`), wraz z kodu serwera i pasujących tagów HTML. Ponownie, można także poprzedzić każdego wiersza za `@:` operator; albo sposób działania.

    > [!NOTE]
    > Podczas drukowania tekstu opisane w tej sekcji &#8212; za pomocą elementu HTML `@:` , operator lub `<text>` element &#8212; Program ASP.NET nie kodowanie HTML dane wyjściowe. (Jak wspomniano wcześniej, ASP.NET zakodować dane wyjściowe wyrażenia kodu serwera i serwera bloki kodu, które są poprzedzone `@`, z wyjątkiem przypadków wymienionych w tej sekcji.)

### <a name="whitespace"></a>Odstępu

Dodatkowe spacje w instrukcji (i poza literału ciągu) nie wpływają na instrukcji:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Podział wiersza w instrukcji nie ma wpływu na instrukcję i może zawijać się instrukcje dla czytelności. Poniższe instrukcje są takie same:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Jednak nie można powielać wiersza środku literału ciągu. Poniższy przykład nie działa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Aby połączyć długi ciąg, który opakowuje do wielu linii, takich jak powyżej kodu, dostępne są dwie opcje. Można używać operatora łączenia (`+`), który pojawi się w dalszej części tego artykułu. Można również użyć `@` znak można utworzyć ciągu dosłownego wyrażenia literału, jak przedstawiono wcześniej w tym artykule. Literały ciągu dosłownego wyrażenia mogą być dzielone między wiersze:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Kod (i znaczników) komentarzy

Komentarze pozwalają pozostaw notatek dla siebie lub innych użytkowników. Umożliwiają również wyłączyć (*komentarz*) sekcji kodu lub kod znaczników, który nie chcesz uruchomić, ale można przechowywać na stronie w chwili obecnej.

Istnieje inny komentowania składnię Razor kodu oraz kod znaczników HTML. Podobnie jak w przypadku wszystkich kodu Razor, Razor komentarze są przetwarzane (i następnie usuwane) na serwerze przed wysłaniem strony do przeglądarki. W związku z tym składnia komentarza Razor umożliwia wprowadzone komentarze do kodu (lub nawet znaczników), które można zobaczyć, kiedy należy edytować plik, ale użytkownicy nie będą widzieli, nawet w źródle strony.

Komentarze ASP.NET Razor start komentarz z `@*` i Zakończ ją z `*@`. Komentarz może być w jednym lub wielu wierszy:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Poniżej przedstawiono komentarz w bloku kodu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

W tym miejscu jest tym samym bloku kodu z wierszem kodu oznaczone jako komentarz tak, aby nie można go uruchomić:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

W bloku kodu zamiast przy użyciu składni komentarza Razor, za pomocą komentowania składni języka programowania, którego używasz, takich jak C#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

W języku C# są poprzedzone komentarz jednowierszowy `//` znaków i Komentarze wielowierszowe zaczynać `/*` się i kończyć `*/`. (Tak jak w przypadku komentarz Razor komentarze języka C# nie są renderowane w przeglądarce.)

W przypadku znaczników prawdopodobnie wiesz, można utworzyć komentarz HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Komentarze HTML rozpoczynać `<!--` znaków i kończyć się `-->`. Komentarze HTML umożliwia przestrzenny nie tylko tekstu, ale również żadnych znaczników HTML, który można przechowywać na stronie, ale nie mają być renderowane. Ten komentarz HTML spowoduje ukrycie całej zawartości znaczniki i tekst, który zawiera:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

W przeciwieństwie do komentarzy Razor, HTML, uwag *są* odwzorowywany na stronie, a użytkownik zobaczy ich wyświetlając źródło strony.

Razor ma ograniczenia w zagnieżdżonych bloków języka C#. Aby uzyskać więcej informacji, zobacz [o nazwie C# zmiennych i zagnieżdżone bloki Generuj uszkodzony kod](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Zmienne

Zmienna jest nazwanego obiektu, który służy do przechowywania danych. Zmienne można nazwy wszystko, ale nazwa musi zaczynać się znakiem alfabetycznym i nie może zawierać spacji ani znaków zastrzeżonych.

### <a name="variables-and-data-types"></a>Typy danych i zmienne

Zmienna może mieć określonego typu danych, co oznacza, jakiego rodzaju dane są przechowywane w zmiennej. Mogą mieć zmiennych ciągu, których są przechowywane wartości ciągów (takich jak &quot;Hello world&quot;), zmienne liczba całkowita, które przechowują liczby całkowitej wartości (na przykład 3 lub 79) i zmienne Data przechowywanie wartości daty w różnych formatach (na przykład 4 12/2012 lub marca 2009 ). I istnieje wiele typów danych, w których można użyć.

Jednak zwykle nie trzeba określać typ zmiennej. W większości przypadków, platformy ASP.NET można ustalić typu, w oparciu sposobu używania danych za pomocą zmiennej. (Czasami musi określać typ; pojawi się przykłady gdzie to PRAWDA).

Należy zadeklarować zmienną za pomocą `var` — słowo kluczowe (Jeśli nie chcesz określić typ) lub przy użyciu nazwy typu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

W poniższym przykładzie przedstawiono niektóre typowe zastosowania zmienne na stronie sieci web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Jeśli znajdzie się na stronie poprzednich przykładach, widzisz taki komunikat wyświetlany w przeglądarce:

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konwertowanie i testowania typy danych

Mimo że program ASP.NET zazwyczaj można automatycznie określić typu danych, czasami nie jest. W związku z tym konieczne może pomóc ASP.NET, wykonując jawnej konwersji. Nawet jeśli nie masz konwersji typów, czasami warto test, aby sprawdzić, jakiego typu dane mogą być pracy z.

Najbardziej często zdarza się, czy masz do przekonwertowania ciągu na inny typ, takich jak do liczby całkowitej lub daty. W poniższym przykładzie przedstawiono typowy przypadek, w którym należy przekonwertować ciąg na liczbę.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Z reguły dane wejściowe użytkownika dostarczany jako ciągi. Nawet jeśli została monitowany użytkownikom wprowadź liczbę z zakresu, a nawet wtedy, gdy ich wprowadzono cyfrę, po przesłaniu danych wejściowych użytkownika, a następnie go odczytać w kodzie, dane są w formacie ciągu. W związku z tym należy przekonwertować ciągu na liczbę. W tym przykładzie podjęcie próby wykonania arytmetycznego na wartościach bez konwertowania, następujący błąd wyników, ponieważ program ASP.NET nie można dodać dwa ciągi:

*Nie można niejawnie przekonwertować typu "string" do "int".*

Aby dokonać konwersji wartości na liczby całkowite, należy wywołać `AsInt` metody. Konwersja zakończy się pomyślnie, można następnie dodać liczb.

W poniższej tabeli wymieniono niektóre typowe metody konwersji i testowania dla zmiennych.

| **— Metoda** | **Opis** | **Przykład** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Konwertuje ciąg reprezentujący liczbę całkowitą (na przykład "593") na liczbę całkowitą. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)] |
| `AsBool(), IsBool()` | Konwertuje ciąg, takich jak &quot;true&quot; lub &quot;false&quot; na typ Boolean. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)] |
| `AsFloat(), IsFloat()` | Konwertuje ciąg o wartości dziesiętnej, takich jak &quot;1.3&quot; lub &quot;7.439&quot; liczby zmiennoprzecinkowej. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)] |
| `AsDecimal(), IsDecimal()` | Konwertuje ciąg o wartości dziesiętnej, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę dziesiętną. (W programie ASP.NET, liczbą dziesiętną jest bardziej dokładne niż liczba zmiennoprzecinkowa). | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)] |
| `AsDateTime(), IsDateTime()` | Konwertuje ciąg reprezentujący wartość daty i godziny do platformy ASP.NET `DateTime` typu. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)] |
| `ToString()` | Konwertuje ciąg inny typ danych. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>Operatory

Operator jest słowo kluczowe lub znak, który informuje ASP.NET, jakiego rodzaju polecenie do wykonania w wyrażeniu. W języku C# (i składni Razor oparty na nim) obsługuje wielu operatorów, ale musisz rozpoznać kilka, aby rozpocząć pracę. Poniższa tabela zawiera podsumowanie typowych operatorów.

| **Operator** | **Opis** | **Przykłady** |
| --- | --- | --- |
| `+``-``*``/` | Operatory matematyczne używać w wyrażeniach numerycznych. | [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)] |
| `=` | Przypisanie. Przypisuje wartości po prawej stronie instrukcji obiektu po lewej stronie. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)] |
| `==` | Równości. Zwraca `true` Jeśli wartości są równe. (Zwróć uwagę, że `=` operatora i `==` operatora.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)] |
| `!=` | Nierówności. Zwraca `true` wartości nie są równe. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)] |
| `< > <= >=` | Mniej-niż większe-niż mniej niż — lub równości i większa niż lub równości. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)] |
| `+` | Łączenie, który jest używany do przyłączenia ciągów. ASP.NET wie, że różnica między Ten operator i operator dodawania na podstawie typu danych wyrażenia. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)] |
| `+=``-=` | Operatory inkrementacji i dekrementacji, które dodawania i odejmowania 1 (odpowiednio) ze zmienną. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)] |
| `.` | Kropki. Pozwala odróżnić obiektów i ich właściwości i metody. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)] |
| `()` | Nawiasy. Używane do wyrażenia grupy oraz do przekazania parametrów do metod. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
| `[]` | Nawiasy kwadratowe. Używane do uzyskiwania dostępu do wartości w macierzy lub kolekcji. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)] |
| `!` | Nie. Odwraca `true` do wartości `false` i na odwrót. Zazwyczaj używany jako sposób skrócona do testowania `false` (oznacza to, aby nie `true`). | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)] |
| `&&`<code>&#124;&#124;</code> | Logiczny AND i lub, w którym są używane do łączenia ze sobą warunki. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)] |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Praca z pliku i ścieżki folderu w kodzie

Ścieżki plików i folderów będzie często współpracować w kodzie. Oto przykład struktury folderu fizycznego dla witryny sieci Web może pojawiać się na komputerze deweloperskim:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Oto niektóre istotne szczegóły dotyczące adresów URL i ścieżki:

- Adres URL rozpoczyna się od jedną nazwę domeny (`http://www.example.com`) lub nazwy serwera (`http://localhost`, `http://mycomputer`).
- Adres URL odnosi się do ścieżki fizycznej na komputerze hosta. Na przykład `http://myserver` może odpowiadać do folderu *C:\websites\mywebsite* na serwerze.
- Ścieżka wirtualna jest skrócona do reprezentowania ścieżek w kodzie bez konieczności określania pełną ścieżkę. Zawiera części adresu URL, który następuje nazwa domeny lub tego serwera. Użycie ścieżki wirtualne, można przenieść do innej domeny lub serwer kodu bez konieczności aktualizacji ścieżki.

Oto przykład, aby ułatwić zrozumienie różnic:

| Pełny adres URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nazwa serwera | *mycompanyserver* |
| Ścieżka wirtualna | */HumanResources/CompanyPolicy.htm* |
| Ścieżka fizyczna | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Wirtualnego katalogu głównego jest /, podobnie jak w folderze głównym dysku c dysku \. (Folder wirtualny ścieżek zawsze używać ukośniki). Ścieżka wirtualna folderu nie musi mieć taką samą nazwę jak folder fizycznych; może to być alias. (Na serwerach produkcyjnych, ścieżka wirtualna rzadko zgodna dokładnej ścieżki fizycznej.)

Podczas pracy z plikami i folderami w kodzie, czasami należy odwoływać się do ścieżki fizycznej, a czasami ścieżki wirtualnej, w zależności od tego, jakie obiekty użytkownik pracuje. ASP.NET udostępnia te narzędzia do pracy z ścieżek plików i folderów w kodzie: `Server.MapPath` metody i `~` operatora i `Href` metody.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Konwertowanie ścieżki wirtualnej do fizycznego: Metoda Server.MapPath

`Server.MapPath` Metoda konwertuje ścieżki wirtualnej (takie jak */default.cshtml*) do bezwzględna ścieżka fizyczna (takich jak *C:\WebSites\MyWebSiteFolder\default.cshtml*). Możesz użyć tej metody w każdej chwili należy pełną ścieżkę fizyczną. Typowym przykładem jest podczas odczytywania lub zapisywania pliku tekstowego lub pliku obrazu na serwerze sieci web.

Zwykle nie wiadomo bezwzględna ścieżka fizyczna witryny na serwerze lokacji hostingu, więc tej metody można przekonwertować ścieżki wiadomo, — ścieżka wirtualna — do odpowiedniego ścieżki na serwerze dla Ciebie. Ścieżka wirtualna są przekazywane do pliku lub folderu do metody i zwraca ścieżkę fizyczną:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odwołanie do wirtualnego katalogu głównego: ~ operatora i Href — metoda

W *.cshtml* lub *.vbhtml* plików, można odwoływać się przy użyciu ścieżka wirtualnego katalogu głównego `~` operatora. Jest to bardzo przydatne, ponieważ można przenosić stron w witrynie, a wszystkie łącza, które zawierają do innych stron będzie przerwane. Możliwe jest również przydatne w przypadku gdy przeniesiesz kiedykolwiek witryny sieci Web w innej lokalizacji. Oto kilka przykładów:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Jeśli witryna sieci Web jest `http://myserver/myapp`, Oto jak ASP.NET, będą traktować te ścieżki po uruchomieniu na stronie:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Te ścieżki faktycznie nie będzie widoczny jako wartości zmiennej, ale ASP.NET będzie traktować ścieżki tak, jakby to, jakie były).

Można użyć `~` operator zarówno w kodzie serwera (jak powyżej), jak i w znaczniku w następujący sposób:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

W znaczniku, użyj `~` operator do tworzenia ścieżek do zasobów, takich jak pliki obrazów, innych stron sieci web i pliki CSS. Po uruchomieniu strony ASP.NET sprawdza za pośrednictwem strony (kod i znaczników) i usuwa wszystkie `~` odwołania do danej ścieżce.

## <a name="conditional-logic-and-loops"></a>Logikę warunkową i pętli

Kodu serwera ASP.NET umożliwia wykonywanie zadań na podstawie warunków i pisania kodu, który powtarza instrukcje określoną liczbę razy (to znaczy kodu wykonywanego w pętli).

### <a name="testing-conditions"></a>Testowanie warunków

Aby przetestować prosty warunek użyjesz `if` instrukcji, która zwraca wartość PRAWDA lub FAŁSZ, oparta na test, możesz określić:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

`if` — Słowo kluczowe uruchamia blok. Rzeczywiste testu (warunek) jest w nawiasach i zwraca wartość PRAWDA lub FAŁSZ. Instrukcje, które są uruchamiane wtedy test ma wartość PRAWDA są ujęte w nawiasy klamrowe. `if` Może zawierać instrukcji `else` bloku, który określa instrukcje do uruchomienia, jeśli wynikiem warunku jest FAŁSZ:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Można dodać wiele warunków przy użyciu `else if` bloku:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

W tym przykładzie, jeśli pierwszy warunek w, gdy blok nie jest prawdziwe, `else if` warunku jest zaznaczony. Jeśli ten warunek jest spełniony, instrukcje w `else if` bloku są wykonywane. Jeśli żaden z warunków nie jest spełniony, instrukcje w `else` bloku są wykonywane. Można dodać dowolną liczbę else if blokuje, a następnie Zamknij z `else` zablokować jako &quot;wszystkich pozostałych&quot; warunku.

Aby przetestować wiele warunków, użyj `switch` bloku:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Wartość do sprawdzenia jest w nawiasach (w tym przykładzie `weekday` zmiennej). Każdy test poszczególnych używa `case` instrukcji, która kończy się średnikiem (;). Jeśli wartość `case` instrukcji jest zgodna z wartością testu, wykonywany jest kod w tym przypadku bloku. Zamknij każdej instrukcji case z `break` instrukcji. (Jeśli zapomnisz do uwzględnienia podziału w każdym `case` bloku kodu z następnej `case` instrukcji uruchomi również.) A `switch` blok często ma `default` instrukcji, jak w przypadku ostatniego &quot;wszystkich pozostałych&quot; opcji, jeśli żaden z innych przypadków nie jest spełniony.

Wynik ostatnich dwóch bloków warunkowych wyświetlany w przeglądarce:

![Razor Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Kod pętli

Często konieczne jest cyklicznie uruchamiany tej samej instrukcji. W tym celu zapętlenia. Na przykład często uruchomieniu same instrukcje dla każdego elementu w kolekcji danych. Jeśli wiesz, dokładnie tak jak często chcesz pętli, możesz użyć `for` pętli. Tego rodzaju pętli jest szczególnie przydatna w przypadku licząc w lub inwentaryzacji:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Rozpoczyna się od pętli `for` — słowo kluczowe następują trzy instrukcje w nawiasach, każdy kończyć się średnikiem.

- Wewnątrz nawiasów, w pierwszej instrukcji (`var i=10;`) licznik tworzy i inicjuje go do 10. Nie masz nazwy licznika `i` &#8212; można użyć dowolnej zmiennej. Gdy `for` pętla zostanie uruchomiona, licznik jest automatycznie zwiększany.
- Drugi — instrukcja (`i < 21;`) ustawia warunek dla jakim ma być liczba. W takim przypadku ma przejść do maksymalnie 20 (oznacza to, Graj dalej podczas licznik jest mniejsza niż 21).
- Trzeci — instrukcja (`i++` ) używa operatora inkrementacji, która po prostu Określa, że licznik powinny mieć do niego dodana każdym uruchomieniu pętli 1.

W nawiasach klamrowych jest kod, który będzie wykonywany dla każdej iteracji pętli. Kod znaczników, tworzy nowy akapit (`<p>` element) każdego czasu i dodaje wiersz danych wyjściowych, wyświetlanie wartość `i` (licznik). Po uruchomieniu tej strony w przykładzie jest tworzony 11 wiersza wyświetlania danych wyjściowych, z tekstem w każdym wierszu wskazującą liczbę elementów.

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

Jeśli pracujesz z kolekcji lub tablicy, często używasz `foreach` pętli. Kolekcja jest grupą podobne obiekty i `foreach` pętli pozwala przeprowadzić zadanie dla każdego elementu w kolekcji. Ten typ pętli jest wygodne dla kolekcji, ponieważ w odróżnieniu od `for` pętli, nie trzeba zwiększyć licznik lub ustawienie limitu. Zamiast tego `foreach` pętli kod wykonywany po prostu za pomocą kolekcji dopiero po jej zakończeniu.

Na przykład następujący kod zwraca elementy `Request.ServerVariables` kolekcji, który jest obiektem, który zawiera informacje o serwerze sieci web. Używa `foreac` h pętli do wyświetlenia nazwy każdego elementu w celu tworzenia nowego `<li>` element na liście punktowanej HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

`foreach` Słowie kluczowym występuje w nawiasach, gdzie zadeklarować zmiennej, która reprezentuje jeden element w kolekcji (w tym przykładzie `var item`), a następnie `in` — słowo kluczowe, a następnie według kolekcji ma pętli. W treści `foreach` pętli, są dostępne bieżącego elementu przy użyciu zgłoszonego wcześniej zmiennej.

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

Aby utworzyć bardziej ogólnego przeznaczenia pętli, użyj `while` instrukcji:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

A `while` pętli rozpoczyna się od `while` — słowo kluczowe, następuje nawiasów której określić, jak długo nadal pętli (tutaj, aby uzyskać tak długo, jak `countNum` jest mniejsza niż 50), następnie bloku powtórzeń. Zwykle zwiększenie pętli (dodać) lub zmniejszenie (odejmowanie) zmiennej lub obiekt używany do zliczania. W tym przykładzie `+=` operator dodaje od 1 do `countNum` każdym uruchomieniu pętli. (Aby zmniejszyć zmiennej w pętli, które zlicza w dół, użyje operator dekrementacji `-=`).

## <a name="objects-and-collections"></a>Obiektów i kolekcji

Prawie wszystko w witrynie sieci Web platformy ASP.NET jest obiektem, w tym strona sieci web. W tej sekcji omówiono niektóre obiekty ważne, często będą korzystać z kodu użytkownika.

### <a name="page-objects"></a>Obiekty strony

Najbardziej podstawowa obiektu w programie ASP.NET jest to strona. Można uzyskać dostępu do właściwości obiektu strony bezpośrednio, bez żadnych obiektów kwalifikujących. Poniższy kod pobiera ścieżkę do pliku, przy użyciu `Request` obiekt strony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Aby była wyczyść że w przypadku odwołuje właściwości i metody dla bieżącego obiektu strony można opcjonalnie użyć słowa kluczowego `this` do reprezentowania obiektów strony w kodzie. Poniżej przedstawiono w poprzednim przykładzie kodu z `this` dodane do reprezentowania strony:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Można użyć właściwości `Page` obiekt, aby pobrać wiele informacji, takich jak:

- `Request`. Jak już przeczytane, to zbiór informacji o bieżącym żądaniu, w tym, jakiego rodzaju przeglądarki zgłosił żądanie, adres URL strony tożsamości użytkownika, itp.
- `Response`. To jest zbiorem informacji na temat odpowiedzi (strona), który będzie wysyłany do przeglądarki, jeśli kod serwera zakończył działanie. Na przykład służy tej właściwości można zapisać informacji do odpowiedzi. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Kolekcja obiektów (tablice i słowników)

A *kolekcji* jest grupy obiektów tego samego typu, takich jak kolekcja `Customer` obiektów z bazy danych. Program ASP.NET zawiera wiele kolekcji wbudowanych, tak samo, jak `Request.Files` kolekcji.

Często będzie współpracować danych w kolekcjach. Istnieją dwa typy kolekcji wspólnej *tablicy* i *słownika*. Tablica jest przydatne, gdy chcesz przechowywanie kolekcji podobnych elementów, ale nie chcesz utworzyć oddzielne zmienną do przechowywania każdego elementu:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Dla tablic, należy zadeklarować określonego typu danych, takich jak `string`, `int`, lub `DateTime`. Aby wskazać, że zmienna może zawierać tablicę, Dodaj nawiasy deklaracji (takich jak `string[]` lub `int[]`). Można uzyskać dostępu do elementów w tablicy przy użyciu ich pozycji (indeks) lub za pomocą `foreach` instrukcji. Indeksy tablicy są liczony od zera &#8212; oznacza to, że pierwszy element jest w pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Można określić liczbę elementów w tablicy, pobierając jego `Length` właściwości. Aby uzyskać położenie konkretny element w tablicy (Aby wyszukać), należy użyć `Array.IndexOf` metody. Użytkownik może również wykonywać następujące czynności wstecznego zawartości tablicy ( `Array.Reverse` — metoda) i sortować zawartość ( `Array.Sort` metody).

Dane wyjściowe kod tablicy ciąg wyświetlany w przeglądarce:

![Razor Img13](introducing-razor-syntax-c/_static/image13.jpg)

Słownik jest kolekcją par klucz/wartość, gdzie Podaj klucz (lub nazwę) można ustawić lub pobrać odpowiadającej jej wartości:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Aby utworzyć słownika, należy użyć `new` — słowo kluczowe, aby wskazać, czy tworzysz nowy obiekt słownika. Słownik można przypisać do zmiennej przy użyciu `var` — słowo kluczowe. Wskazuje typy danych elementów w słowniku przy użyciu nawiasu ostrego ( `< >` ). Na końcu deklaracji należy dodać parę nawiasów, ponieważ jest to rzeczywiście metodę, która tworzy nowy słownik.

Aby dodać elementy do słownika, należy wywołać `Add` metody na zmienną słownika (`myScores` w takim przypadku), a następnie określ klucz i wartość. Alternatywnie można nawiasy kwadratowe wskazuje klucz i wykonaj przypisanie proste, jak w poniższym przykładzie:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Wywoływanie metody z parametrami

Omówione w tym artykule, obiekty, które program z może mieć metody. Na przykład `Database` obiekt może mieć `Database.Connect` metody. Wiele metod również mieć co najmniej jeden parametr. A *parametru* jest wartością, który jest przekazywany do metody Aby włączyć metodę zakończy się. Na przykład przyjrzeć się deklaracji `Request.MapPath` metodę, która przyjmuje trzy parametry:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Linia jest opakowana aby był bardziej czytelny. Należy pamiętać, że podziałów niemal dowolnym miejscu, z wyjątkiem wewnątrz ciągów, które można umieścić są ujęte w cudzysłów.)

Ta metoda zwraca ścieżkę fizyczną na serwerze, który odpowiada do określonej ścieżki wirtualnej. Są trzy parametry dla metody `virtualPath`, `baseVirtualDir`, i `allowCrossAppMapping`. (Zwróć uwagę, że w deklaracji, parametry są wyświetlane z typami danych dane, które akceptują będzie). Gdy ta metoda jest wywoływana, należy podać wartości parametrów wszystkich trzech.

Składnia Razor oferuje dwie opcje parametrów może być przekazywany do metody: *parametrów pozycyjnych* i *parametrów nazwanych*. Aby wywołać metodę przy użyciu parametrów pozycyjnych, parametry przekazywane w kolejności strict, który jest określony w deklaracji metody. (Będzie zazwyczaj znasz to zamówienie przeczytaj dokumentację metody.) Wykonaj kolejności i nie można pominąć wszystkie parametry &#8212; Jeśli to konieczne, możesz przekazać pusty ciąg (`""`) lub `null` dla parametrów pozycyjnych, które nie mają wartości.

W poniższym przykładzie założono istnieje folder o nazwie *skryptów* w witrynie sieci Web. Kod wywołuje `Request.MapPath` — metoda i przekazuje wartości parametrów trzy we właściwej kolejności. Wyświetla zamapowanej ścieżki wynikowej.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Gdy metoda ma wiele parametrów, można zachować swój kod był bardziej czytelny przy użyciu parametrów nazwanych. Aby wywołać metodę przy użyciu parametrów nazwanych, należy określić nazwę parametru, po której następuje dwukropkiem (:) i wartość. Zaletą nazwane parametry jest, że można przekazać je w dowolnej kolejności, która ma. (Wadą jest wywołanie metody nie jest możliwie kompaktowego).

Poniższy przykład wywołuje tę samą metodę jak powyżej, ale używa nazwanych parametrów umożliwiają określanie wartości:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Jak widać, parametry są przekazywane w innej kolejności. Jednak po uruchomieniu poprzedni przykład, jak i w tym przykładzie zostanie zwrócona taką samą wartość.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Obsługa błędów

### <a name="try-catch-statements"></a>Instrukcje try-Catch

Konieczne będzie często instrukcje w kodzie, który może zakończyć się niepowodzeniem ze względów poza formantu. Na przykład:

- W przypadku kodu próbuje utworzyć lub uzyskać dostępu do pliku, wszystkie rodzaje błędów mogą wystąpić. Żądany plik nie istnieje, może być zablokowane, kod może nie mieć uprawnień i tak dalej.
- Podobnie jeśli kod spróbuje do aktualizowania rekordów w bazie danych, mogą być problemy z uprawnieniami, mogą być opuszczane połączenia z bazą danych, można zapisać danych może być nieprawidłowa i tak dalej.

W terminologii programistycznej tych sytuacji są nazywane *wyjątki*. Jeśli kod napotkał wyjątek, generuje (zgłasza) komunikat o błędzie to jest w najlepszym irytujących dla użytkowników:

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

W sytuacjach, gdy kod może wystąpić wyjątki, a w celu uniknięcia komunikaty o błędach tego typu, można użyć `try/catch` instrukcje. W `try` instrukcji, uruchom kod, który jest sprawdzanie. W co najmniej jednej `catch` instrukcji, można wyszukać określonych błędów (określonych typów wyjątków), które mogły wystąpić. Może zawierać tyle `catch` instrukcje jako użytkownik musiał błędów, które są przewidywania.

> [!NOTE]
> Zaleca się unikać używania `Response.Redirect` metoda `try/catch` instrukcji, ponieważ może spowodować wyjątek na stronie.


W poniższym przykładzie przedstawiono strona, która tworzy plik tekstowy na pierwsze żądanie, a następnie wyświetla przycisku, który umożliwia użytkownikowi otwieranie pliku. W przykładzie użyto celowo Nieprawidłowa nazwa pliku, aby go spowoduje, że wystąpił wyjątek. Kod zawiera `catch` instrukcji na dwa wyjątki to możliwe: `FileNotFoundException`, który występuje, jeśli nazwa pliku jest nieprawidłowa, i `DirectoryNotFoundException`, które występuje, gdy ASP.NET nawet nie można odnaleźć folderu. (Użytkownik może usuń znaczniki komentarza do instrukcji w tym przykładzie aby zobaczyć, jak jest uruchamiana po wszystko działa prawidłowo.)

Jeśli kod nie obsłużyć wyjątek, zobaczysz stronę błędu, takie jak poprzednie zrzut ekranu. Jednak `try/catch` sekcja pomoże uniemożliwienia użytkownika tego typu błędów.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

**Programowania w języku Visual Basic**


[Dodatku: język Visual Basic i składni](https://go.microsoft.com/fwlink/?LinkId=202908)


**Dokumentacji**


[ASP.NET](https://msdn.microsoft.com/en-us/library/ee532866.aspx)

[C# — język](https://msdn.microsoft.com/en-us/library/kx37x362.aspx)
