---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (Visual Basic) | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten dodatek zapewnia przegląd programowania, korzystając z wzorca ASP.NET Web pages w języku Visual Basic z użyciem składni Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: d4be7e2ed1b847d8b4167872728815330dbfe432
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378744"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Wprowadzenie do programowania dla sieci Web platformy ASP.NET używająca składni Razor (Visual Basic)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule umożliwia przegląd programowania przy użyciu stron ASP.NET Web Pages przy użyciu składni Razor i Visual Basic. ASP.NET to technologia firmy Microsoft dotyczące uruchamiania dynamicznych stron sieci web na serwerach sieci web.
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


Większość przykładów przy użyciu stron ASP.NET Web Pages o składni Razor używaj języka C#. Ale języka Visual Basic obsługuje również składni Razor. Programowanie na stronie sieci web platformy ASP.NET w języku Visual Basic, utworzyć stronę sieci web w taki sposób, przy użyciu *.vbhtml* rozszerzenie nazwy pliku, a następnie dodać kod języka Visual Basic. Ten artykuł zawiera omówienie pracy z języka Visual Basic i składnię tworzenia stron sieci Web ASP.NET.

> [!NOTE]
> Domyślne szablony witryny sieci Web programu Microsoft WebMatrix (**dla piekarni**, **Galeria fotografii**, i **witryny początkowej**, itp.) są dostępne w wersjach C# i Visual Basic. Szablony programu Visual Basic, można zainstalować jako pakiety NuGet. Szablonów sieci Web są zainstalowane w folderze głównym lokacji w folderze o nazwie *Templates firmy Microsoft*.


## <a name="the-top-8-programming-tips"></a>Najważniejsze 8 porady dotyczące programowania

W tej sekcji przedstawiono kilka wskazówek, które bezwzględnie musisz wiedzieć, po rozpoczęciu pisania kodu serwera ASP.NET używająca składni Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Możesz dodać kod do strony przy użyciu znaku @

`@` Znak rozpoczyna się w tekście wyrażeń, bloków pojedynczej instrukcji i bloków wielu instrukcji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Kodowanie HTML**
> 
> Podczas wyświetlania zawartości na stronie za pomocą `@` znaków, jak w poprzednich przykładach, ASP.NET koduje jako HTML dane wyjściowe. Spowoduje to zastąpienie zastrzeżone znaki HTML (takie jak `<` i `>` i `&`) przy użyciu kodów, umożliwiające znaków, które mają być wyświetlane jako znaki na stronie sieci web, a interpretowany jako tagów HTML lub jednostki. Zakodowany w formacie HTML, dane wyjściowe z kodu serwera mogą być wyświetlane nieprawidłowo i spowodować narażenie na zagrożenia bezpieczeństwa strony.
> 
> Jeśli dowiesz się, jak dane wyjściowe kod znaczników HTML, który renderuje tagi jako kod znaczników (na przykład `<p></p>` akapitu lub `<em></em>` aby wyróżnić tekst), zobacz sekcję [łączenie tekstu, znaczników i kodu w blokach kodu](#BM_CombiningTextMarkupAndCode) w dalszej części tego artykułu.
> 
> Możesz dowiedzieć się więcej o kodowanie HTML w [Praca z formularzami HTML w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Umieść bloki kodu przy użyciu kodu... Kod zakończenia

Blok kodu zawiera jeden lub więcej instrukcji kodu i znajduje się za pomocą słów kluczowych `Code` i `End Code`. Umieść otwierający `Code` — słowo kluczowe natychmiast po `@` znak &#8212; nie może zawierać odstępów między nimi.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Wewnątrz bloku zakończenia każda instrukcja kodu za pomocą podział wiersza

W bloku kodu języka Visual Basic każda instrukcja kończy się podział wiersza. (W dalszej części tego artykułu zobaczysz sposób opakować instrukcję kodu długa na wiele wierszy, jeśli to konieczne.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. W przypadku używania zmiennych do przechowywania wartości

Można przechowywać wartości w *zmiennej*, w tym ciągi, liczby i daty, itp. Możesz utworzyć nową zmienną za pomocą `Dim` — słowo kluczowe. Wartości zmiennych można wstawić bezpośrednio na stronie za pomocą `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Ujmij wartości literału ciągu w znaki podwójnego cudzysłowu

A *ciąg* jest sekwencją znaków, które są traktowane jako tekst. Aby określić ciąg, ująć w znaki cudzysłowu:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Aby osadzić podwójnego cudzysłowu wewnątrz wartości ciągu, Wstaw dwa znaki cudzysłowu. Znak cudzysłowu się jeden raz w danych wyjściowych strony, wprowadź go w formie `""` w cudzysłowie ciąg i się pojawić się dwa razy, wprowadź go jako `""""` w ramach ciągów w cudzysłowach.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Kod języka Visual Basic nie jest uwzględniana wielkość liter

Język Visual Basic nie jest uwzględniana wielkość liter. Programowanie słowa kluczowe (takie jak `Dim`, `If`, i `True`) i nazwy zmiennych (takich jak `myString`, lub `subTotal`) mogą być zapisywane w każdym przypadku.

Następujące wiersze kodu przypisać wartość do zmiennej `lastname` przy użyciu małymi literami nazwę, a następnie wyprowadzić wartość zmiennej strony za pomocą wielkie nazwy.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Wynik jest wyświetlany w przeglądarce:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Większość kodowania wymaga pracy z obiektami

Obiekt reprezentuje rzecz, którą można programować za pomocą &#8212; strony pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta (wiersz bazy danych), itp. Obiekty mają właściwości, które opisują ich właściwości &#8212; ma obiekt pola tekstowego `Text` właściwości obiektu żądania ma `Url` właściwość, wiadomość e-mail ma `From` właściwość i obiekt klienta ma `FirstName` Właściwość. Obiektów ma także metody, które są &quot;zleceń&quot; mogą wykonywać. Przykłady obejmują obiekt pliku `Save` metodę, obiekt obrazu `Rotate` metody i obiekt e-mail `Send` metody.

Często będziesz pracować `Request` obiektu, który zapewnia informacje, takie jak wartości formularza pola na stronie (pola tekstowe itp.), jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp. W tym przykładzie przedstawiono sposób uzyskiwania dostępu do właściwości `Request` obiektu i wywoływania `MapPath` metody `Request` obiektu, który zapewnia ścieżkę bezwzględną strony na serwerze:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Można napisać kod, który podejmuje decyzje

Kluczową funkcją dynamicznych stron sieci web jest, czy można określić, co należy zrobić, na podstawie warunków. Jest najbardziej popularny sposób, w tym celu `If` — instrukcja (i opcjonalnie `Else` instrukcji).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Wykonywanie instrukcji `If IsPost` jest skrót sposobem pisania `If IsPost = True`. Wraz z `If` instrukcji, istnieje wiele sposobów, aby przetestować warunki, powtórz bloki kodu, i itd., które są opisane w dalszej części tego artykułu.

Wynik wyświetlany w przeglądarce (po kliknięciu przycisku **przesyłania**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET i POST metod i właściwości IsPost**
> 
> Protokół używany dla stron sieci web (HTTP) obsługuje bardzo ograniczona liczba metod (&quot;zleceń&quot;) które są używane, aby wysyłać żądania do serwera. Dwie najbardziej typowe to GET, który służy do odczytu strony i WPIS, który jest używany do przesyłania strony. Ogólnie rzecz biorąc po raz pierwszy użytkownik zgłasza żądanie strony, strony jest przesyłane przy użyciu GET. Jeśli użytkownik wypełnia formularz, a następnie klika przycisk **przesyłania**, przeglądarce wysyła żądanie POST do serwera.
> 
> W programowanie dla sieci web jest często grupowaniu można sprawdzić, czy strona jest wymagana jako GET lub POST, aby znać sposób przetwarzania tej strony. W składniku ASP.NET Web Pages można użyć `IsPost` właściwość, aby sprawdzić, czy żądanie GET lub POST. Jeśli żądanie jest żądaniem POST `IsPost` właściwość zostanie zwrócona wartość PRAWDA, a można wykonywać takie czynności, takich jak odczyt wartości pola tekstowe w formularzu. Wiele przykładów zobaczysz pokazują, jak można przetworzyć strony inaczej w zależności od wartości `IsPost`.


## <a name="a-simple-code-example"></a>Prosty przykład kodu

Ta procedura pokazuje, jak utworzyć stronę, która przedstawia podstawowe techniki programowania. W tym przykładzie utworzysz stronę, która umożliwia użytkownikom wprowadź dwie liczby, a następnie dodanie ich i wyświetla wynik.

1. W edytorze, Utwórz nowy plik i nadaj mu nazwę *AddNumbers.vbhtml*.
2. Skopiuj następujący kod i znaczników do strony, zastępując wszystko już na stronie.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Oto kilka rzeczy, umożliwiające należy zwrócić uwagę:

    - `@` Znak rozpoczyna się pierwszego bloku kodu na stronie i poprzedza ono `totalMessage` zmiennej osadzone w dolnej części.
    - Blok, w górnej części strony jest ujęty w `Code...End Code`.
    - Zmienne `total`, `num1`, `num2`, i `totalMessage` przechowywać kilka liczb i ciąg.
    - Wartość literału ciągu przypisana do `totalMessage` zmiennej znajduje się w znaki podwójnego cudzysłowu.
    - Ponieważ kod języka Visual Basic nie jest uwzględniana wielkość liter, kiedy `totalMessage` zmienna jest używana w dolnej części strony, jego nazwa tylko musi być zgodna pisownię deklaracja zmiennej w górnej części strony. Wielkość liter w wyrazie nie ma znaczenia.
    - Wyrażenie `num1.AsInt()`  +  `num2.AsInt()` pokazuje, jak pracować z obiektami i metody. `AsInt` Metody dla każdej zmiennej konwertuje ciąg wprowadzanego przez użytkownika do liczb całkowitych (liczba całkowita), który można dodać.
    - `<form>` Znacznik obejmuje `method="post"` atrybutu. Określa to, że gdy użytkownik kliknie **Dodaj**, strony, które będą wysyłane do serwera przy użyciu metody POST protokołu HTTP. Po przesłaniu strony, kod `If IsPost` daje w wyniku wartość PRAWDA, a warunkowy kod działa, wyświetlanie wynikiem dodania liczb.
3. Zapisz stronę i uruchom go w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.) Wprowadź dwoma liczbami całkowitymi, a następnie kliknij przycisk **Dodaj** przycisku.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Język Visual Basic i składnia

Wcześniej był wyświetlany podstawowy przykład sposobu tworzenia strony sieci web ASP.NET i jak możesz dodać kod serwera do kod znaczników HTML. Tutaj dowiesz się podstawowe informacje dotyczące pisania kodu serwera ASP.NET używająca składni Razor przy użyciu języka Visual Basic &#8212; czyli programowania reguły języka.

Jeśli masz doświadczenie w pracy z programowania (zwłaszcza, jeśli używano C, C++, C#, Visual Basic lub JavaScript), większość tutaj przeczytaj będą niczym nowym. Prawdopodobnie należy zapoznać się tylko z jak kod w programie WebMatrix jest dodawany do znaczników w *.vbhtml* plików.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Łączenie tekstu, znaczników i kodu w blokach kodu

W blokach kodu serwera należy często danych wyjściowych tekst i znacznik do strony. Jeśli blok kodu serwera zawiera tekst nie jest kodem, który zamiast tego powinien być renderowany jako jest, ASP.NET musi być w stanie odróżnić go od kodu. Istnieje kilka sposobów, aby to zrobić.

- Należy wpisać tekst w elemencie bloku HTML, takich jak `<p></p>` lub `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML element może zawierać tekstu, dodatkowe elementy HTML i wyrażenia w kodzie serwera. Gdy program ASP.NET widzi otwierający HTML tag (na przykład `<p>`), wszystko, czego renderowania elementu a jej zawartość jako przeglądarki (i rozwiązuje wyrażeń kodu serwera).

- Użyj `@:` operatora lub `<text>` elementu. `@:` Generuje pojedynczy wiersz zawartość zawierająca zwykły tekst lub niedopasowane tagów HTML; `<text>` element zawiera wiele wierszy w danych wyjściowych. Te opcje są przydatne, jeśli nie chcesz renderować HTML element jako część danych wyjściowych.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    W poniższym przykładzie jest powtarzany w poprzednim przykładzie, ale używa jedna para `<text>` tagów, aby ująć tekstu do renderowania.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    W poniższym przykładzie `<text>` i `</text>` tagi należy ująć w trzy wiersze, które mają niektóre niedopasowane tagi HTML i uncontained tekst (`<br />`), wraz z kodu serwera i dopasowane tagów HTML. Ponownie, również może poprzedzać każdy wiersz za pomocą `@:` operator; albo sposób działania.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Gdy tekstu wyjściowego przedstawione w tej sekcji &#8212; przy użyciu elementu HTML `@:` operatora lub `<text>` elementu &#8212; ASP.NET nie kodowanie HTML dane wyjściowe. (Jak wspomniano wcześniej, ASP.NET, kodowania danych wyjściowych wyrażeń kodu serwera i serwera bloków kodu, które są poprzedzone `@`, z wyjątkiem specjalnych przypadków wymienionych w tej sekcji.)

### <a name="whitespace"></a>Białe znaki

Dodatkowe spacje w instrukcji (i poza literału ciągu), nie mają wpływu na instrukcji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Podzielenie długie instrukcje na wiele wierszy

Instrukcja długie kodu można podzielić wiele wierszy za pomocą znaku podkreślenia `_` (w języku Visual Basic nazywana *znak kontynuacji*) po każdym wierszu kodu. Aby przerwać instrukcję do następnego wiersza, na końcu wiersza dodać spację, a następnie znak kontynuacji. Continue — instrukcja w następnym wierszu. Może zawijać się instrukcje, na których trzeba poprawić czytelność dowolną liczbę wierszy. Poniższe instrukcje są takie same:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Jednak nie można opakować linię trakcie literału ciągu. Poniższy przykład nie działa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Aby połączyć długi ciąg, który jest zawijany do wielu linii, takich jak kod powyżej, czy należy użyć *operatora łączenia* (`&`), które zostaną wyświetlone w dalszej części tego artykułu.

### <a name="code-comments"></a>Komentarze w kodzie

Komentarze umożliwiają pozostawienie notatki dla siebie i innych osób. Komentarze składni razor mają prefiks `@*` i kończyć się znakiem `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Wewnątrz bloków kodu można użyć komentarze składni Razor, lub można użyć zwykły znak komentarz języka Visual Basic, który jest pojedynczy cudzysłów (`'`) prefiks do każdego wiersza.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Zmienne

Zmienna jest nazwany obiekt, który służy do przechowywania danych. Zmienne możesz nazwać wszystko, ale nazwa musi rozpoczynać się od litery alfabetu i nie może zawierać spacji ani znaków zarezerwowanych. W języku Visual Basic, jak przedstawiono wcześniej, wielkość liter w nazwie zmiennej nie ma znaczenia.

### <a name="variables-and-data-types"></a>Zmienne i typy danych

Zmienna może mieć na określony typ danych, która wskazuje, jakiego typu dane są przechowywane w zmiennej. Mogą mieć zmiennych ciągu, które przechowują wartości ciągu (takich jak &quot;Witaj, świecie&quot;), zmiennych całkowitych, które przechowują wartości liczby całkowitej (np. 3 lub 79) i zmienne daty, które przechowywanie wartości daty w różnych formatach (np. 4/12/2012 lub marca 2009 ). Wiąże się wiele typów danych, w których można użyć.

Jednakże nie trzeba określać typu zmiennej. W większości przypadków platformy ASP.NET można ustalić typu, w oparciu sposobu korzystania z danych w zmiennej. (Czasami musisz określić typ; pojawi się przykłady którym ta zasada obowiązuje).

Aby zadeklarować zmienną bez określania typu, należy użyć `Dim` oraz nazwę zmiennej (na przykład `Dim myVar`). Aby zadeklarować zmienną typu, należy użyć `Dim` oraz nazwę zmiennej, a następnie `As` i następnie nazwę typu (na przykład `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Poniższy przykład przedstawia niektóre wbudowane wyrażeń zmiennych na stronie sieci web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Wynik jest wyświetlany w przeglądarce:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konwertowanie i testowanie typów danych

Mimo że program ASP.NET zazwyczaj może automatycznie określić typu danych, czasami nie jest. W związku z tym konieczne może być pomógł ASP.NET, wykonując jawnej konwersji. Nawet jeśli nie masz konwersji typów, czasami warto sprawdzić, jakiego typu dane być może pracujesz z.

Najbardziej często zdarza się, trzeba przekonwertować ciąg do innego typu, takie jak liczbą całkowitą lub daty. Poniższy przykład przedstawia typową sytuacją, w którym należy przekonwertować ciąg na liczbę.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Zgodnie z zasadą dane wejściowe użytkownika dostarczany jako ciągi. Nawet jeśli została monituje użytkownika o wprowadzenie numeru, a nawet wtedy, gdy zostały wprowadzone cyfr, podczas przesyłania danych wejściowych użytkownika, a następnie go odczytać w kodzie, dane są w formacie ciągu. W związku z tym należy przekonwertować ciąg na liczbę. W przykładzie Jeśli zostanie podjęta próba wykonania operacji arytmetycznych na wartościach, bez konwersji, następujący błąd wyników, ponieważ program ASP.NET nie można dodać dwa ciągi:

`Cannot implicitly convert type 'string' to 'int'.`

Aby przekonwertować wartości liczb całkowitych, należy wywołać `AsInt` metody. Jeśli konwersja się pomyślnie, następnie można dodać liczb.

Poniższa lista zawiera niektóre typowe metody konwersji i testowania dla zmiennych.


::: wiersz:::::: kolumny::: <strong>metoda</strong> ::: koniec kolumny:::::: kolumny::: <strong>opis</strong> ::: koniec kolumny:::::: kolumny::: <strong>przykład</strong> ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsInt(), IsInt()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, który reprezentuje liczbę całkowitą z zakresu (np. &quot;593&quot;) na liczbę całkowitą.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsBool(), IsBool()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, takich jak &quot;true&quot; lub &quot;false&quot; na typ Boolean.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsFloat(), IsFloat()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, który ma wartość dziesiętną, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę zmiennoprzecinkową.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsDecimal(), IsDecimal()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg, który ma wartość dziesiętną, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę dziesiętną. (W programie ASP.NET: liczba dziesiętna jest bardziej precyzyjne niż liczba zmiennoprzecinkowa). ::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AsDateTime(), IsDateTime()` ::: koniec kolumny:::::: kolumny::: Konwertuje ciąg reprezentujący wartość daty i godziny ASP.NET `DateTime` typu.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `ToString()` ::: koniec kolumny:::::: kolumny::: jakimkolwiek innym typem danych jest konwertowany na ciąg.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    ::: koniec kolumny:::::: końca wiersza:::


## <a name="operators"></a>Operatory

Operator jest słowo kluczowe lub znak, który informuje ASP.NET, jakiego rodzaju polecenie do wykonania w wyrażeniu. Visual Basic obsługuje wiele operatorów, ale musisz rozpoznać kilka, aby rozpocząć wdrażanie stron ASP.NET web pages. Poniższa tabela zawiera podsumowanie najbardziej typowych operatorów.


::: wiersz:::::: kolumny::: <strong>Operator</strong> ::: koniec kolumny:::::: kolumny::: <strong>opis</strong> ::: koniec kolumny:::::: kolumny::: <strong>przykłady</strong> ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `+ - * /` ::: koniec kolumny:::::: kolumny::: operatorów matematycznych używać w wyrażeniach liczbowych.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `=` ::: koniec kolumny:::::: kolumny::: przypisania i równości. W zależności od kontekstu przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie, albo sprawdza, czy wartości dla równości.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `<>` ::: koniec kolumny:::::: kolumny::: nierówności. Zwraca `True` wartości nie są równe.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `< > <= >=` ::: koniec kolumny:::::: kolumny::: mniejsze niż, większe niż, mniejsze lub równe i większa niż lub równe.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `&` ::: koniec kolumny:::::: kolumny::: łączenia, który jest używany do łączenia ciągów.
::: koniec kolumny:::::: kolumny::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `+= -=` ::: koniec kolumny:::::: kolumny::: operatorów inkrementacji i dekrementacji, które dodawania i odejmowania 1 (odpowiednio) ze zmiennej.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `.` ::: koniec kolumny:::::: kolumny::: kropka. Używany do odróżnienia obiektów i ich właściwości i metody.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `()` ::: koniec kolumny:::::: kolumny::: nawiasów. Używane do wyrażenia grupy w celu przekazania parametrów do metod i dostęp do elementów członkowskich tablice i kolekcje.
::: koniec kolumny:::::: kolumny::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `Not` ::: koniec kolumny:::::: kolumny::: nie. Odwraca wartość true na false i odwrotnie. Zwykle jest używana jako sposób skrót do testowania `False` (oznacza to, aby nie `True`).
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    ::: koniec kolumny:::::: końca wiersza:::
* * *
::: wiersz:::::: kolumny::: `AndAlso OrElse` ::: koniec kolumny:::::: kolumny::: logiczne i i lub które są używane do łączenia ze sobą warunki.
::: koniec kolumny:::::: kolumny::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    ::: koniec kolumny:::::: końca wiersza:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odwoływanie się do wirtualnego katalogu głównego: ~ operatora i Href, metoda

W *.cshtml* lub *.vbhtml* pliku, możesz odwoływać się za pomocą ścieżka wirtualnego katalogu głównego `~` operatora. Może to być bardzo przydatne, ponieważ możesz poruszać się strony w witrynie, a wszystkie łącza, które zawierają do innych stron nie będzie działać. Jest to również przydatne w przypadku, gdy przeniesiesz kiedykolwiek witryny sieci Web w innej lokalizacji. Oto kilka przykładów:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Jeśli witryna sieci Web `http://myserver/myapp`, Oto jak ASP.NET będzie traktować te ścieżki, po uruchomieniu na stronie:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Te ścieżki faktycznie nie będzie widoczna jako wartości zmiennej, ale ASP.NET traktują ścieżki, tak, jakby to, jakie były).

Możesz użyć `~` operator zarówno w kodzie serwera (jak powyżej), jak i znaczników w następujący sposób:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

W znaczniku, użyj `~` operatora do utworzenia ścieżki do zasobów, takich jak pliki obrazów, innych stron sieci web i plików CSS. Po uruchomieniu strony ASP.NET przegląda strony (kodu i znaczników) i usuwa wszystkie `~` odwołania do danej ścieżce.

## <a name="conditional-logic-and-loops"></a>Logika warunkowe i pętle

Kod serwera ASP.NET umożliwia wykonywanie zadań na podstawie warunków i zapisu kod powtarzany instrukcji określonej liczby razy oznacza to, że kod uruchamiany pętli).

### <a name="testing-conditions"></a>Warunki badania

Aby przetestować warunku prostego, możesz użyć `If...Then` instrukcję, która zwraca `True` lub `False` na podstawie testu, można określić:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If` — Słowo kluczowe uruchamia blok. Test rzeczywisty (stan) jest zgodna `If` — słowo kluczowe i zwraca wartość PRAWDA lub FAŁSZ. `If` Instrukcja kończy się `Then`. Znajdują się wewnątrz instrukcji, które zostanie uruchomione, gdy test jest true `If` i `End If`. `If` Może zawierać instrukcji `Else` blok, który określa instrukcje do uruchomienia, jeśli warunek nie jest spełniony:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Jeśli `If` instrukcji rozpoczyna blok kodu, nie trzeba stosować normalną `Code...End Code` instrukcji, aby zawierała bloki. Można dodać `@` do bloku, i będzie ona działać. Ta metoda działa z `If` oraz innych programowania słów kluczowych, które następują bloków kodu, w tym Visual Basic `For`, `For Each`, `Do While`itp.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Można dodać wiele warunków przy użyciu co najmniej jeden `ElseIf` bloków:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

W tym przykładzie, jeśli pierwszy warunek w `If` bloku nie ma wartość true, `ElseIf` warunek jest sprawdzany. Jeśli ten warunek jest spełniony, instrukcje w `ElseIf` bloku są wykonywane. Jeśli żaden z warunków nie jest spełniony, instrukcje w `Else` bloku są wykonywane. Można dodać dowolną liczbę `ElseIf` blokuje, a następnie Zamknij z `Else` zablokować, jako &quot;cała reszta&quot; warunku.

Aby przetestować dużą liczbę warunków, należy użyć `Select Case` bloku:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Wartość do sprawdzenia jest w nawiasach (w przykładzie zmienna dzień tygodnia). Każdy pojedynczy test używa `Case` instrukcję, która zawiera listę wartości. Jeśli wartość `Case` instrukcji pasuje do wartości testu, kod w tym `Case` blok jest wykonywany.

Wynik ostatnich dwóch bloków warunkowych, wyświetlany w przeglądarce:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Tworzenie pętli kodu

Często muszą uruchomić te same instrukcje wielokrotnie. W tym pętli są uwzględnione. Na przykład można często uruchamiane te same instrukcje dla każdego elementu w kolekcji danych. Jeśli wiesz, dokładnie tak jak często chcesz pętli, możesz użyć `For` pętli. Ten rodzaj pętli jest szczególnie przydatna podczas liczenia w lub inwentaryzacji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Pętla zaczyna się od `For` — słowo kluczowe następują trzy elementy:

- Natychmiast po `For` instrukcji, Zadeklaruj zmienną licznika (nie trzeba stosować `Dim`), a następnie zaznaczyć zakres, podobnie jak w `i = 10 to 20`. Oznacza to, że zmienna `i` uruchomi zliczania, 10 i kontynuować, dopóki nie osiągnie 20 (włącznie).
- Między `For` i `Next` instrukcji znajduje się odpowiednia zawartość bloku. To może zawierać jedną lub więcej kodu instrukcji, które są wykonywane w każdej iteracji.
- `Next i` Instrukcja kończy pętli. On zwiększa licznik i uruchamia następnej iteracji pętli.

W wierszu kodu między `For` i `Next` wierszy zawiera kod, który jest wykonywany dla każdej iteracji pętli. Znaczniki, tworzy nowy akapit (`<p>` elementu) każdego czas i dodaje wiersz danych wyjściowych, wyświetlanie wartości i (licznik). Po uruchomieniu tej strony, ten przykład tworzy 11 wiersza, wyświetlanie danych wyjściowych z tekstem w każdym wierszu, wskazującą liczbę elementów.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Jeśli pracujesz z kolekcji lub tablicy, są często używane `For Each` pętli. Kolekcja to grupa podobnych obiektów i `For Each` pętli pozwala przeprowadzić zadanie dla każdego elementu w kolekcji. Ten typ pętli jest wygodne w przypadku kolekcji, ponieważ w odróżnieniu od `For` pętli, nie trzeba ten licznik lub Ustaw limit. Zamiast tego `For Each` kodu pętli po prostu obejmującego kolekcji dopiero po jej zakończeniu.

W tym przykładzie zwraca elementy w `Request.ServerVariables` kolekcję (która zawiera informacje o serwerze sieci web). Używa ona `For Each` pętli, aby wyświetlić nazwę każdego elementu przez utworzenie nowego `<li>` element na liście punktowanej we wcześniejszej HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each` — Słowo kluczowe następuje zmienną, która reprezentuje pojedynczy element w kolekcji (w tym przykładzie `myItem`), a następnie `In` — słowo kluczowe, następuje kolekcję ma pętli. W treści `For Each` pętli, możesz uzyskać dostęp bieżącego elementu przy użyciu zmiennej zadeklarowanej wcześniej.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Aby utworzyć bardziej ogólnego przeznaczenia pętli, użyj `Do While` instrukcji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Ta pętla zaczyna się od `Do While` — słowo kluczowe warunku, a następnie bloku, aby powtórzyć. Zazwyczaj Zwiększ pętli (dodać) lub dekrementacji (odjęcia od) zmiennej lub obiektu użytego do zliczania. W tym przykładzie `+=` operator dodaje 1 wartość zmiennej przy każdym uruchomieniu pętli. (W celu dekrementacja zmiennej w pętlę, który odlicza, należy użyć operatora dekrementacyjnego `-=`.)

## <a name="objects-and-collections"></a>Obiekty i kolekcje

Prawie wszystko w witrynie internetowej platformy ASP.NET jest obiektem, w tym strona sieci web. W tej sekcji omówiono niektóre ważne obiekty, które często będziesz pracować z w kodzie.

### <a name="page-objects"></a>Obiekty strony

Najbardziej podstawowa obiektu w programie ASP.NET: jest to strona. Możesz uzyskać dostęp właściwości obiektu strony bezpośrednio, bez dowolnych obiektów kwalifikujących. Poniższy kod pobiera ścieżkę do pliku, przy użyciu `Request` obiekt strony:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Można użyć właściwości `Page` można uzyskać mnóstwo informacji, takich jak:

- `Request`. Jak już przeczytane, to zbiór informacji o bieżącym żądaniu, w tym, jakiego rodzaju przeglądarki wysłał żądanie, adres URL strony, tożsamość użytkownika, itp.
- `Response`. Jest to zbiór informacji na temat odpowiedzi (strona), który będzie wysyłany do przeglądarki, gdy kod serwera zakończył działanie. Na przykład można używasz tej właściwości można zapisać informacji do odpowiedzi.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Obiekty kolekcji (tablic i słowników)

Kolekcja to grupa obiektów tego samego typu, na przykład zbiór `Customer` obiektów z bazy danych. Program ASP.NET zawiera wiele kolekcji wbudowanych, takie jak `Request.Files` kolekcji.

Często będziesz pracować z danymi w kolekcji. Istnieją dwa typy kolekcji typowe *tablicy* i *słownika*. Tablica jest przydatne, gdy ma być przechowywany w kolekcji podobnych elementów, ale nie chcesz utworzyć osobne zmienną do przechowywania każdego elementu:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

W przypadku tablic, możesz zadeklarować na określony typ danych, takich jak `String`, `Integer`, lub `DateTime`. Aby wskazać, że zmienna może zawierać tablicę, Dodaj nawiasy do nazwy zmiennej w deklaracji (takie jak `Dim myVar() As String`). Możesz uzyskać dostęp elementów w tablicy przy użyciu ich pozycji (indeks) lub za pomocą `For Each` instrukcji. Indeksy tablicy są oparte na zerze &#8212; oznacza to, pierwszy element jest na pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Można określić liczbę elementów w tablicy, uzyskując jego `Length` właściwości. Można pobrać pozycja konkretny element w tablicy (oznacza to, aby wyszukać), użyj `Array.IndexOf` metody. Można również wykonać elementów, takich jak wstecznego zawartość tablicy ( `Array.Reverse` metoda) i sortować zawartość ( `Array.Sort` metody).

Dane wyjściowe kodu tablicy ciąg wyświetlany w przeglądarce:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Słownik to kolekcja par klucz wartość, określane klucz (lub nazwę) można ustawić lub pobrać odpowiednią wartość:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Aby utworzyć słownik, należy użyć `New` — słowo kluczowe, aby wskazać, czy tworzysz nową `Dictionary` obiektu. Słownik można przypisać do zmiennej za pomocą `Dim` — słowo kluczowe. Możesz określać typy danych elementów w słowniku przy użyciu nawiasów ( `( )` ). Na końcu deklaracji należy dodać kolejną parę nawiasów, ponieważ jest to rzeczywiście metodę, która tworzy nowy słownik.

Aby dodać elementy do słownika, możesz wywołać `Add` metody na zmienną słownika (`myScores` w tym przypadku), a następnie określ klucz i wartość. Alternatywnie można użyć nawiasów do klucza, a następnie wykonaj przypisanie proste, jak w poniższym przykładzie:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Wywołanie metody z parametrami

Jak przedstawiono wcześniej w tym artykule obiektów, które program za pomocą mają metody. Na przykład `Database` obiekt może mieć `Database.Connect` metody. Wiele metod również mieć co najmniej jeden parametr. A *parametru* jest wartością, który jest przekazywany do metody, aby włączyć metodę w celu zakończenia zadania. Na przykład Przyjrzyj się deklarację dla `Request.MapPath` metody, która przyjmuje trzy parametry:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Ta metoda zwraca ścieżkę fizyczną na serwerze, który odnosi się do określonej ścieżki wirtualnej. Są trzy parametry dla metody `virtualPath`, `baseVirtualDir`, i `allowCrossAppMapping`. (Zwróć uwagę, że w deklaracji, parametry są wyświetlane z typami danych, który będzie mógł zaakceptować). Po wywołaniu tej metody należy podać wartości dla wszystkich trzech parametrów.

Podczas korzystania z języka Visual Basic z użyciem składni Razor, masz dwie opcje do przekazywania parametrów do metody: *parametry pozycyjne* lub *nazwanych parametrów*. Aby wywołać metodę, przy użyciu parametry pozycyjne, parametry są przekazywane w kolejności strict, który jest określony w deklaracji metody. (Będzie zazwyczaj wiesz to zamówienie, przeczytaj dokumentację dla metody.) Należy wykonać, kolejność i nie można pominąć dowolny z parametrów &#8212; Jeśli to konieczne, możesz przekazać pusty ciąg (`""`) lub wartość null parametru pozycyjnych, które nie mają wartości.

W poniższym przykładzie założono, masz folder o nazwie *skrypty* w witrynie sieci Web. Kod wywołuje `Request.MapPath` metody i przekazuje wartości dla trzech parametrów w odpowiedniej kolejności. Następnie wyświetla ścieżki wynikowej zamapowany.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Gdy istnieje wiele parametrów dla metody, można zachować kod bardziej przejrzyste i bardziej czytelny przy użyciu nazwanych parametrów. Aby wywołać metodę, przy użyciu nazwanych parametrów, należy określić nazwę parametru, a następnie `:=` , a następnie wprowadź wartość. Zaletą nazwanych parametrów jest, możesz je dodać w dowolnej kolejności, które chcesz. (Wadą jest wywołanie metody nie jest najmniejszy).

Poniższy przykład wywołuje tę samą metodę jak powyżej, ale korzysta z nazwanych parametrów, aby podać wartości:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Jak widać, parametry są przekazywane w innej kolejności. Jednak po uruchomieniu poprzedniego przykładu i w tym przykładzie zostanie zwrócona taką samą wartość.

## <a name="handling-errors"></a>Obsługa błędów

### <a name="try-catch-statements"></a>Instrukcje try-Catch

Często masz instrukcji w kodzie, który może zakończyć się niepowodzeniem ze względów poza Twoją kontrolą. Na przykład:

- Jeśli kod próbuje otworzyć, tworzenia, odczytu lub zapisu pliku, mogą wystąpić wszelkiego rodzaju błędów. Żądany plik nie istnieje, może być zablokowana, kod może nie mieć uprawnień i tak dalej.
- Podobnie jeśli kod próbuje zaktualizować rekordy w bazie danych, może to być problemy z uprawnieniami, mogą być opuszczane połączenia z bazą danych, dane w celu zaoszczędzenia może być nieprawidłowy i tak dalej.

W sposób programowania, są nazywane tych sytuacji *wyjątki*. Jeśli Twój kod napotka wyjątek, generuje (zgłasza) komunikatu o błędzie oznacza to, co najlepiej irytujących dla użytkowników.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

W sytuacjach, w którym kod może wystąpić wyjątki i w celu uniknięcia komunikaty o błędach tego typu można użyć `Try/Catch` instrukcji. W `Try` instrukcji, uruchom kod, który jest sprawdzany. W co najmniej jednej `Catch` instrukcji, można wyszukać konkretnego błędów (określonych typów wyjątków), które mogły wystąpić. Może zawierać tyle `Catch` instrukcje, jak należy sprawdzić występowanie błędów, które są przewidywania.

> [!NOTE]
> Firma Microsoft zaleca, unikaj używania `Response.Redirect` method in Class metoda `Try/Catch` instrukcji, ponieważ może to spowodować wyjątek na stronie.


Poniższy przykład pokazuje stronę, która tworzy plik tekstowy na pierwsze żądanie, a następnie wyświetla przycisk, który umożliwia użytkownikowi, otwórz plik. W przykładzie użyto celowo Zła nazwa pliku, tak aby spowoduje wyjątek. Kod zawiera `Catch` instrukcji dla dwóch możliwych wyjątków: `FileNotFoundException`, który występuje, jeśli nazwa pliku jest nieprawidłowa, i `DirectoryNotFoundException`, który występuje, gdy program ASP.NET nawet nie można odnaleźć folderu. (Aby zobaczyć, jak działa, gdy wszystko będzie działać prawidłowo można komentarz do instrukcji w tym przykładzie).

Jeśli Twój kod nie obsługuje wyjątek, widział takich jak poprzedniej zrzut ekranu przedstawiający stronę błędu. Jednak `Try/Catch` sekcji uniemożliwia użytkownikowi widzisz błędy tych typów.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

### <a name="reference-documentation"></a>Dokumentacja referencyjna

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Język Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
