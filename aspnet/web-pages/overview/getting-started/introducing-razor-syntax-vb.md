---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor (Visual Basic) | Dokumentacja firmy Microsoft
author: tfitzmac
description: Ten dodatek umożliwia omówienie programowania w języku strony sieci Web ASP.NET w języku Visual Basic, przy użyciu składni Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 715e52715fb22b92f94d3d602ec58c29a913426c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Wprowadzenie do programowania sieci Web ASP.NET przy użyciu składni Razor (Visual Basic)
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule zapewnia przegląd programowania ze strony sieci Web ASP.NET przy użyciu składni Razor i Visual Basic. Program ASP.NET to technologia firmy Microsoft do uruchamiania dynamicznych stron sieci web na serwerach sieci web.
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
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


Większość przykładów przy użyciu stron ASP.NET Web Pages o składni Razor Użyj C#. Jednak ze składni Razor obsługuje również Visual Basic. Do programowania w języku Visual Basic strony sieci web ASP.NET, tworzenia strony sieci web z *.vbhtml* rozszerzenie nazwy pliku, a następnie dodaj kod Visual Basic. W tym artykule przedstawiono omówienie pracy z języka Visual Basic i tworzenia stron sieci Web ASP.NET.

> [!NOTE]
> Domyślne szablony witryny sieci Web programu Microsoft WebMatrix (**Bakery**, **galerii fotografii**, i **witryny początkowej**, itd.) są dostępne w wersjach C# i Visual Basic. Szablony programu Visual Basic przez można zainstalować jako pakietów NuGet. Szablony witryny sieci Web są instalowane w folderze głównym lokacji w folderze o nazwie *Templates Microsoft*.


## <a name="the-top-8-programming-tips"></a>Górny 8 porady dotyczące programowania

W tej sekcji przedstawiono kilka wskazówek, które należy bezwzględnie znać rozpoczęcie pisania kodu serwera ASP.NET przy użyciu składni Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Dodaj kod do strony przy użyciu znaku @

`@` Znak uruchamia wyrażenia w tekście, pojedynczą instrukcją bloki, a wielu instrukcji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Wynik wyświetlony w przeglądarce:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Kodowanie HTML**
> 
> Podczas wyświetlania zawartości na stronie za pomocą `@` znak w powyższych przykładach ASP.NET HTML-koduje dane wyjściowe. Spowoduje to zastąpienie zarezerwowanych znaków HTML (takich jak `<` i `>` i `&`) z kodami umożliwiających znaków, które mają być wyświetlane jako znaki na stronie sieci web, a nie interpretowany jako tagów HTML lub jednostek. Zakodowany w formacie HTML, dane wyjściowe w kodzie serwera może nie wyświetlać się poprawnie, a może spowodować narażenie strony na zagrożenia bezpieczeństwa.
> 
> Celem użytkownika jest output kod znaczników HTML, który renderuje tagi jako kod znaczników (na przykład `<p></p>` akapitu lub `<em></em>` aby podkreślić tekst), zobacz sekcję [łączenie tekstu, znaczników i kodu w bloki kodu](#BM_CombiningTextMarkupAndCode) dalszej części tego artykułu.
> 
> Więcej o kodowanie HTML w [Praca z formularzy HTML w witrynach stron sieci Web platformy ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Należy ująć bloków kodu za pomocą kodu... Kod zakończenia:

Blok kodu zawiera jedną lub więcej instrukcji kodu i znajduje się ze słowami kluczowymi `Code` i `End Code`. Umieść otwierający `Code` — słowo kluczowe natychmiast po `@` znak &#8212; nie może być spacji między nimi.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Wynik wyświetlony w przeglądarce:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. W bloku kończyć każda instrukcja kodu z podział wiersza

W bloku kodu języka Visual Basic każda instrukcja kończy podział wiersza. (W dalszej części tego artykułu zobaczysz sposób do opakowywania instrukcję kodu długa na wiele wierszy, jeśli to konieczne.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. W przypadku używania zmiennych do przechowywania wartości

Można zapisać wartości w *zmiennej*, w tym ciągów, liczb i daty, itp. Można utworzyć nowej zmiennej przy użyciu `Dim` — słowo kluczowe. Można wstawić wartości zmiennych bezpośrednio za pomocą strony `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Wynik wyświetlony w przeglądarce:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. To ująć w podwójny cudzysłów wartości literału ciągu

A *ciąg* jest sekwencji znaków, które są traktowane jako tekst. Aby określić ciąg, ująć w podwójny cudzysłów:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Aby osadzić podwójny cudzysłów wewnątrz wartości ciągu, Wstaw dwa znaki cudzysłowu. Znak cudzysłowu pojawią się raz w danych wyjściowych strony, należy wprowadzić go w formie `""` w cudzysłowie ciągu, a jeśli ma być wyświetlany dwa razy, wprowadź go jako `""""` ciągu ujętego w cudzysłów.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Wynik wyświetlony w przeglądarce:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Kod Visual Basic nie jest uwzględniana wielkość liter

Język Visual Basic nie jest uwzględniana wielkość liter. Słowa kluczowe programowania (takich jak `Dim`, `If`, i `True`) i nazwy zmiennych (takich jak `myString`, lub `subTotal`) mogą być zapisywane w każdym przypadku.

Następujące wiersze kodu przypisać wartości do zmiennej `lastname` przy użyciu małych nazwy i dane wyjściowe wartości zmiennej do strony przy użyciu nazwy wielkie litery.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Wynik wyświetlony w przeglądarce:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Większość kodowania obejmuje pracy z obiektami

Obiekt reprezentuje element, który zostanie z &#8212; strony, pola tekstowego, pliku, obraz, żądania sieci web, wiadomości e-mail, rekord klienta (wiersza bazy danych), itp. Obiekty mają właściwości, które opisują ich właściwości &#8212; ma obiekt pola tekstowego `Text` właściwość, obiekt żądania ma `Url` właściwość, wiadomości e-mail ma `From` właściwości oraz obiektu klienta ma `FirstName` Właściwość. Obiekty mają również metody, które są &quot;zleceń&quot; mogą wykonywać. Przykładami obiektu pliku `Save` metodę, obiekt obrazu `Rotate` — metoda i obiektu poczty e-mail `Send` metody.

Często będzie współpracować `Request` obiektu, który zawiera informacje, takie jak wartości formularza pola na stronie (pola tekstowe itp.), jaki typ przeglądarki zgłosił żądanie, adres URL strony tożsamości użytkownika, itp. W tym przykładzie pokazano, jak uzyskać dostępu do właściwości `Request` obiektów i wywoływania `MapPath` metody `Request` obiektu, który daje użytkownikowi bezwzględna ścieżka strony na serwerze:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Wynik wyświetlony w przeglądarce:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Można napisać kod, który podejmowania decyzji w procesie

Kluczową funkcją dynamicznej stron sieci web jest, że można określić, co zrobić, na podstawie warunków. Najczęściej w tym celu jest z `If` instrukcji (i opcjonalnie `Else` instrukcji).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Instrukcja `If IsPost` jest skrócona sposobem zapisu `If IsPost = True`. Wraz z programem `If` instrukcji, istnieje wiele sposobów, aby przetestować warunki, powtórz bloki kodu, i itd., które są opisane w dalszej części tego artykułu.

Wynik wyświetlony w przeglądarce (po kliknięciu przycisku **przesyłania**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET i POST metod i właściwości IsPost**
> 
> Protokół używany do stron sieci web (HTTP) obsługuje bardzo ograniczoną liczbę metod (&quot;zleceń&quot;) służące do wysyłać żądania do serwera. Dwa najbardziej typowe to GET, który jest używany do odczytu stronę, i POST, który jest używany do przesyłania strony. Ogólnie rzecz biorąc po raz pierwszy użytkownik zażąda strony, strony jest wymagany przy użyciu GET. Jeśli użytkownik wpisuje do formularza, a następnie klika przycisk **przesyłania**, przeglądarka wysyła żądanie POST do serwera.
> 
> W programowanie dla sieci web często jest grupowaniu można sprawdzić, czy strona jest wymagany jako GET lub POST, aby wiedzieć, jak przetwarzania tej strony. W składniku ASP.NET Web Pages można użyć `IsPost` właściwości, aby zobaczyć, czy żądanie jest GET lub POST. Jeśli żądanie jest żądaniem POST `IsPost` właściwości zwróci wartość true, a użytkownik może wykonywać następujące czynności odczytu wartości pól tekstowych w formularzu. Wiele przykładów zobaczysz opisano sposób przetwarzania strony inaczej w zależności od wartości `IsPost`.


## <a name="a-simple-code-example"></a>Prosty przykład kodu

Tej procedury przedstawiono sposób tworzenia strony, która ilustruje podstawowych technik programowania. W tym przykładzie utworzysz strony, który pozwala użytkownikom na wprowadzanie dwóch liczb, a następnie dodanie ich i wyświetla wyniki.

1. W edytorze, Utwórz nowy plik i nadaj jej nazwę *AddNumbers.vbhtml*.
2. Skopiuj następujący kod i znaczników do strony, zastępując wszystko już na stronie.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Poniżej przedstawiono niektóre czynności umożliwiające należy zwrócić uwagę:

    - `@` Znak rozpoczyna się pierwszego bloku kodu na stronie i poprzedza `totalMessage` zmiennej osadzonych u dołu.
    - Blok w górnej części strony jest ujęta w `Code...End Code`.
    - Zmienne `total`, `num1`, `num2`, i `totalMessage` przechowywać wiele numerów i ciąg.
    - Wartość literału ciągu przypisane do `totalMessage` zmienna jest w podwójny cudzysłów.
    - Ponieważ kod Visual Basic nie jest uwzględniana wielkość liter, gdy `totalMessage` zmienna jest używana w dolnej części strony, jego nazwa tylko musi być zgodna pisownię deklaracja zmiennej w górnej części strony. Wielkość liter nie ma znaczenia.
    - Wyrażenie `num1.AsInt()`  +  `num2.AsInt()` pokazano, jak pracować z obiektów i metod. `AsInt` Metody dla każdej zmiennej konwertuje ciąg wprowadzonej przez użytkownika na liczbę całkowitą (liczba całkowita), który można dodać.
    - `<form>` Zawiera tag `method="post"` atrybutu. Oznacza to, że gdy użytkownik kliknie **Dodaj**, strony zostaną wysłane do serwera przy użyciu metody POST protokołu HTTP. Po przesłaniu strony, kod `If IsPost` daje w wyniku wartość true i warunkowe kod działa wyświetlania wyniku dodawania liczb.
3. Strony i uruchom go w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.) Wprowadź dwie liczb całkowitych, a następnie kliknij przycisk **Dodaj** przycisku.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Język Visual Basic i składni

Widać wcześniej podstawowy przykład sposobu tworzenia strony sieci web platformy ASP.NET i jak można dodać kod serwera do kod znaczników HTML. Tutaj dowiesz się podstawowe informacje dotyczące pisania kodu serwera ASP.NET przy użyciu składni Razor przy użyciu języka Visual Basic &#8212; czyli programowania reguł języka.

Jeśli masz doświadczenia w pracy z programowania (zwłaszcza, jeśli używano C, C++, C#, Visual Basic lub JavaScript), większość tutaj odczytu jest znane. Prawdopodobnie należy zaznajomić się tylko z jak program WebMatrix kodu jest dodawana do kodu znaczników w *.vbhtml* plików.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Łączenie tekstu, znaczników i kodu w bloki kodu

W blokach kodu serwera należy często output tekst i znacznik ze stroną. Jeśli blok kodu serwera zawiera tekst, który nie jest kodem i który zamiast tego ma być renderowany jako jest, ASP.NET musi mieć możliwość rozróżnienia ten tekst z kodu. Istnieje kilka sposobów, aby to zrobić.

- Tekst należy ująć w elemencie bloku HTML, takie jak `<p></p>` lub `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    HTML element mogą zawierać tekst, dodatkowe elementy HTML i wyrażenia kod serwera. Gdy ASP.NET widzi otwierający HTML tag (na przykład `<p>`), wszystko renderowania elementu i jego zawartości jako polega na przeglądarce (i jest rozpoznawana wyrażenia kod serwera).

- Użyj `@:` operator lub `<text>` elementu. `@:` Generuje pojedynczy wiersz zawartości zawierającej zwykłego tekstu lub niedopasowane znaczniki HTML; `<text>` elementu obejmuje wiele wierszy danych wyjściowych. Te opcje są przydatne, jeśli nie chcesz do renderowania elementu HTML jako część danych wyjściowych.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    W poniższym przykładzie jest powtarzany poprzednim przykładzie, ale używa jednej pary `<text>` znaczniki, aby umieścić tekst do renderowania.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    W poniższym przykładzie `<text>` i `</text>` tagi należy ująć w trzy wiersze, które mają niektóre uncontained tekst i niedopasowane znaczniki HTML (`<br />`), wraz z kodu serwera i pasujących tagów HTML. Ponownie, można także poprzedzić każdego wiersza za `@:` operator; albo sposób działania.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Podczas drukowania tekstu opisane w tej sekcji &#8212; za pomocą elementu HTML `@:` , operator lub `<text>` elementu &#8212; ASP.NET nie kodowanie HTML dane wyjściowe. (Jak wspomniano wcześniej, ASP.NET zakodować dane wyjściowe wyrażenia kodu serwera i serwera bloki kodu, które są poprzedzone `@`, z wyjątkiem przypadków wymienionych w tej sekcji.)

### <a name="whitespace"></a>Odstępu

Dodatkowe spacje w instrukcji (i poza literału ciągu) nie wpływają na instrukcji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Fundamentalne długi instrukcje do wielu linii

Instrukcję kodu długo można podzielić wielu wierszy za pomocą znaku podkreślenia `_` (w języku Visual Basic nazywany *znak kontynuacji*) po każdym wierszu kodu. Aby przerwać instrukcję do następnego wiersza na koniec wiersza dodać miejsce, a następnie znak kontynuacji. Continue — instrukcja w następnym wierszu. Można zawijać instrukcje na musisz poprawić czytelność dowolną liczbę wierszy. Poniższe instrukcje są takie same:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Jednak nie można powielać wiersza środku literału ciągu. Poniższy przykład nie działa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Aby połączyć długi ciąg, który opakowuje do wielu linii, takich jak powyżej kodu, należy użyć *operatora łączenia* (`&`), który pojawi się w dalszej części tego artykułu.

### <a name="code-comments"></a>Komentarze w kodzie

Komentarze pozwalają pozostaw notatek dla siebie lub innych użytkowników. Komentarze składni razor mają przedrostek `@*` się i kończyć `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Wewnątrz bloków kodu można użyć składni Razor komentarze lub skorzystać z zwykłej Visual Basic komentarz znak pojedynczego cudzysłowu (`'`) i jest poprzedzony do każdego wiersza.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Zmienne

Zmienna jest nazwanego obiektu, który służy do przechowywania danych. Zmienne można nazwy wszystko, ale nazwa musi zaczynać się znakiem alfabetycznym i nie może zawierać spacji ani znaków zastrzeżonych. W języku Visual Basic jak przedstawiono wcześniej, nie ma znaczenia wielkość liter w nazwie zmiennej.

### <a name="variables-and-data-types"></a>Zmienne i typy danych

Zmienna może mieć określonego typu danych, co oznacza, jakiego rodzaju dane są przechowywane w zmiennej. Mogą mieć zmiennych ciągu, których są przechowywane wartości ciągów (takich jak &quot;Hello world&quot;), zmienne liczba całkowita, które przechowują liczby całkowitej wartości (na przykład 3 lub 79) i zmienne Data przechowywanie wartości daty w różnych formatach (na przykład 4 12/2012 lub marca 2009 ). I istnieje wiele typów danych, w których można użyć.

Jednak nie trzeba określać typ zmiennej. W większości przypadków ASP.NET można ustalić typu, w oparciu sposobu używania danych za pomocą zmiennej. (Czasami musi określać typ; pojawi się przykłady gdzie to PRAWDA).

Aby zadeklarować zmiennej bez określania typu, należy użyć `Dim` oraz nazwę zmiennej (na przykład `Dim myVar`). Aby zadeklarować zmiennej typu, należy użyć `Dim` oraz nazwę zmiennej, a następnie `As` , a następnie wpisz nazwę (na przykład `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

W poniższym przykładzie przedstawiono niektóre wbudowane wyrażeń zmienne na stronie sieci web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Wynik wyświetlony w przeglądarce:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konwertowanie i testowania typy danych

Mimo że program ASP.NET zazwyczaj można automatycznie określić typu danych, czasami nie jest. W związku z tym konieczne może pomóc ASP.NET, wykonując jawnej konwersji. Nawet jeśli nie masz konwersji typów, czasami warto test, aby sprawdzić, jakiego typu dane mogą być pracy z.

Najbardziej często zdarza się, czy masz do przekonwertowania ciągu na inny typ, takich jak do liczby całkowitej lub daty. W poniższym przykładzie przedstawiono typowy przypadek, w którym należy przekonwertować ciąg na liczbę.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Z reguły dane wejściowe użytkownika dostarczany jako ciągi. Nawet jeśli została monituje użytkownika o wprowadzenie numeru, a nawet wtedy, gdy została wprowadzona cyfr, po przesłaniu danych wejściowych użytkownika, a następnie go odczytać w kodzie, dane są w formacie ciągu. W związku z tym należy przekonwertować ciągu na liczbę. W tym przykładzie podjęcie próby wykonania arytmetycznego na wartościach bez konwertowania, następujący błąd wyników, ponieważ program ASP.NET nie można dodać dwa ciągi:

`Cannot implicitly convert type 'string' to 'int'.`

Aby dokonać konwersji wartości na liczby całkowite, należy wywołać `AsInt` metody. Konwersja zakończy się pomyślnie, można następnie dodać liczb.

W poniższej tabeli wymieniono niektóre typowe metody konwersji i testowania dla zmiennych.


|   <strong>— Metoda</strong>    |                                                                              <strong>Opis</strong>                                                                              |                     <strong>Przykład</strong>                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
|      `AsInt(), IsInt()`      |                                                 Konwertuje ciąg reprezentujący liczbę całkowitą z zakresu (takie jak &quot;593&quot;) na liczbę całkowitą.                                                 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)] |
|     `AsBool(), IsBool()`     |                                                    Konwertuje ciąg, takich jak &quot;true&quot; lub &quot;false&quot; na typ Boolean.                                                     | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)] |
|    `AsFloat(), IsFloat()`    |                                    Konwertuje ciąg o wartości dziesiętnej, takich jak &quot;1.3&quot; lub &quot;7.439&quot; liczby zmiennoprzecinkowej.                                    | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)] |
|  `AsDecimal(), IsDecimal()`  | Konwertuje ciąg o wartości dziesiętnej, takich jak &quot;1.3&quot; lub &quot;7.439&quot; na liczbę dziesiętną. (W programie ASP.NET, liczbą dziesiętną jest bardziej dokładne niż liczba zmiennoprzecinkowa). | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)] |
| `AsDateTime(), IsDateTime()` |                                                Konwertuje ciąg reprezentujący wartość daty i godziny do platformy ASP.NET `DateTime` typu.                                                 | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)] |
|         `ToString()`         |                                                                       Konwertuje ciąg inny typ danych.                                                                        | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)] |

## <a name="operators"></a>Operatory

Operator jest słowo kluczowe lub znak, który informuje ASP.NET, jakiego rodzaju polecenie do wykonania w wyrażeniu. Visual Basic obsługuje wielu operatorów, ale musisz rozpoznać kilka, aby rozpocząć tworzenie stron sieci web programu ASP.NET. Poniższa tabela zawiera podsumowanie typowych operatorów.


| <strong>Operator</strong> |                                                                        <strong>Opis</strong>                                                                         |                         <strong>Przykłady</strong>                         |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
|         `+ - * /`         |                                                                Operatory matematyczne używać w wyrażeniach numerycznych.                                                                |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]     |
|            `=`            | Przypisanie i równości. W zależności od kontekstu przypisuje wartość po prawej stronie instrukcji do obiektu po lewej stronie, lub sprawdza wartości pod kątem równości. |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]     |
|           `<>`            |                                                           Nierówności. Zwraca `True` wartości nie są równe.                                                           |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]     |
|        `< > <= >=`        |                                                   Mniej niż większe niż, mniejsze niż lub równy, a mniejsza.                                                   |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]     |
|            `&`            |                                                                Łączenie, który jest używany do przyłączenia ciągów.                                                                | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)] |
|          `+= -=`          |                                       Operatory inkrementacji i dekrementacji, które dodawania i odejmowania 1 (odpowiednio) ze zmienną.                                       |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]     |
|            `.`            |                                                     Kropki. Pozwala odróżnić obiektów i ich właściwości i metody.                                                      |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]     |
|           `()`            |                           Nawiasy. Używany do wyrażenia grupy do przekazania parametrów do metod i do dostępu do członków kolekcji i tablic.                           | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)] |
|           `Not`           |                    Nie. Odwraca wartość true, FALSE i odwrotnie. Zazwyczaj używany jako sposób skrócona do testowania `False` (oznacza to, aby nie `True`).                     |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]     |
|     `AndAlso OrElse`      |                                                       Logiczny AND i lub, w którym są używane do łączenia ze sobą warunki.                                                       |     [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]     |

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
| Ścieżka wirtualna | */humanresources/CompanyPolicy.htm* |
| Ścieżka fizyczna | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Wirtualnego katalogu głównego jest /, podobnie jak w folderze głównym dysku c dysku \. (Folder wirtualny ścieżek zawsze używać ukośniki). Ścieżka wirtualna folderu nie musi mieć taką samą nazwę jak folder fizycznych; może to być alias. (Na serwerach produkcyjnych, ścieżka wirtualna rzadko zgodna dokładnej ścieżki fizycznej.)

Podczas pracy z plikami i folderami w kodzie, czasami należy odwoływać się do ścieżki fizycznej, a czasami ścieżki wirtualnej, w zależności od tego, jakie obiekty użytkownik pracuje. ASP.NET udostępnia te narzędzia do pracy z ścieżek plików i folderów w kodzie: `Server.MapPath` metody i `~` operatora i `Href` metody.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Konwertowanie ścieżki wirtualnej do fizycznego: Metoda Server.MapPath

`Server.MapPath` Metoda konwertuje ścieżki wirtualnej (takie jak */default.cshtml*) do bezwzględna ścieżka fizyczna (takich jak *C:\WebSites\MyWebSiteFolder\default.cshtml*). Możesz użyć tej metody w każdej chwili należy pełną ścieżkę fizyczną. Typowym przykładem jest podczas odczytywania lub zapisywania pliku tekstowego lub pliku obrazu na serwerze sieci web.

Zwykle nie wiadomo bezwzględna ścieżka fizyczna witryny na serwerze lokacji hostingu, więc tej metody można przekonwertować ścieżki wiadomo, — ścieżka wirtualna — do odpowiedniego ścieżki na serwerze dla Ciebie. Ścieżka wirtualna są przekazywane do pliku lub folderu do metody i zwraca ścieżkę fizyczną:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Odwołanie do wirtualnego katalogu głównego: ~ operatora i Href — metoda

W *.cshtml* lub *.vbhtml* plików, można odwoływać się przy użyciu ścieżka wirtualnego katalogu głównego `~` operatora. Jest to bardzo przydatne, ponieważ można przenosić stron w witrynie, a wszystkie łącza, które zawierają do innych stron będzie przerwane. Możliwe jest również przydatne w przypadku gdy przeniesiesz kiedykolwiek witryny sieci Web w innej lokalizacji. Oto kilka przykładów:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Jeśli witryna sieci Web jest `http://myserver/myapp`, Oto jak ASP.NET, będą traktować te ścieżki po uruchomieniu na stronie:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Te ścieżki faktycznie nie będzie widoczny jako wartości zmiennej, ale ASP.NET będzie traktować ścieżki tak, jakby to, jakie były).

Można użyć `~` operator zarówno w kodzie serwera (jak powyżej), jak i w znaczniku w następujący sposób:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

W znaczniku, użyj `~` operator do tworzenia ścieżek do zasobów, takich jak pliki obrazów, innych stron sieci web i pliki CSS. Po uruchomieniu strony ASP.NET sprawdza za pośrednictwem strony (kod i znaczników) i usuwa wszystkie `~` odwołania do danej ścieżce.

## <a name="conditional-logic-and-loops"></a>Logikę warunkową i pętli

Kodu serwera ASP.NET umożliwia wykonywanie zadań na podstawie warunków i zapisu kod który powtarza instrukcje określoną liczbę razy oznacza to, że kod uruchamiany w pętli).

### <a name="testing-conditions"></a>Testowanie warunków

Aby przetestować prosty warunek użyjesz `If...Then` instrukcja, która zwraca `True` lub `False` na podstawie testu, należy określić:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

`If` — Słowo kluczowe uruchamia blok. Rzeczywiste testu (warunek) jest zgodna `If` — słowo kluczowe i zwraca wartość PRAWDA lub FAŁSZ. `If` Kończy instrukcji `Then`. Znajdują się wewnątrz instrukcji, które zostanie uruchomione, gdy test jest true `If` i `End If`. `If` Może zawierać instrukcji `Else` bloku, który określa instrukcje do uruchomienia, jeśli wynikiem warunku jest FAŁSZ:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Jeśli `If` instrukcji rozpoczyna blok kodu, nie trzeba używać normalnej `Code...End Code` instrukcje, aby zawierała bloki. Można dodać `@` blok, i będzie ona działać. Ta metoda działa z `If` oraz innych programowania słów kluczowych, które zostaną wykonane przez bloki kodu, w tym Visual Basic `For`, `For Each`, `Do While`itp.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Można dodać wiele warunków przy użyciu co najmniej jednego `ElseIf` bloków:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

W tym przykładzie, jeśli pierwszy warunek w `If` bloku nie jest prawdziwe, `ElseIf` warunku jest zaznaczony. Jeśli ten warunek jest spełniony, instrukcje w `ElseIf` bloku są wykonywane. Jeśli żaden z warunków nie jest spełniony, instrukcje w `Else` bloku są wykonywane. Można dodać dowolną liczbę `ElseIf` blokuje, a następnie Zamknij z `Else` zablokować jako &quot;wszystkich pozostałych&quot; warunku.

Aby przetestować wiele warunków, użyj `Select Case` bloku:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Wartość do sprawdzenia jest w nawiasach (w przykładzie zmienna dzień tygodnia). Każdy test poszczególnych używa `Case` instrukcji, która zawiera wartości. Jeśli wartość `Case` instrukcji jest zgodna z wartością testu, kod w tym `Case` bloku jest wykonywana.

Wynik ostatnich dwóch bloków warunkowych wyświetlany w przeglądarce:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Kod pętli

Często konieczne jest cyklicznie uruchamiany tej samej instrukcji. W tym celu zapętlenia. Na przykład często uruchomieniu same instrukcje dla każdego elementu w kolekcji danych. Jeśli wiesz, dokładnie tak jak często chcesz pętli, możesz użyć `For` pętli. Tego rodzaju pętli jest szczególnie przydatna w przypadku licząc w lub inwentaryzacji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Rozpoczyna się od pętli `For` — słowo kluczowe, a następnie trzy elementy:

- Natychmiast po `For` instrukcja, deklaruje zmienną licznika (nie trzeba używać `Dim`), a następnie określić zakres, podobnie jak w `i = 10 to 20`. Oznacza to, że zmienna `i` uruchomi, licząc od 10 i kontynuować dopóki nie osiągnie 20 (włącznie).
- Między `For` i `Next` instrukcje znajduje się odpowiednia zawartość bloku. Ten plik może zawierać co najmniej jeden kod instrukcji, które wykonania w każdej iteracji.
- `Next i` Instrukcji kończy się pętli. Zwiększa licznik, a uruchamia następnej iteracji pętli.

Wiersz kodu między `For` i `Next` wierszy zawiera kod, który jest uruchamiana dla każdej iteracji pętli. Kod znaczników, tworzy nowy akapit (`<p>` element) każdego czasu i dodaje wiersz danych wyjściowych, wyświetlanie wartości i (licznik). Po uruchomieniu tej strony w przykładzie jest tworzony 11 wiersza wyświetlania danych wyjściowych, z tekstem w każdym wierszu wskazującą liczbę elementów.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Jeśli pracujesz z kolekcji lub tablicy, często używasz `For Each` pętli. Kolekcja jest grupą podobne obiekty i `For Each` pętli pozwala przeprowadzić zadanie dla każdego elementu w kolekcji. Ten typ pętli jest wygodne dla kolekcji, ponieważ w odróżnieniu od `For` pętli, nie trzeba zwiększyć licznik lub ustawienie limitu. Zamiast tego `For Each` pętli kod wykonywany po prostu za pomocą kolekcji dopiero po jej zakończeniu.

W tym przykładzie zwraca elementy `Request.ServerVariables` kolekcji (zawierający informacje o serwerze sieci web). Używa `For Each` pętli w celu wyświetlenia nazwy poszczególnych elementów, tworząc nowe `<li>` element na liście punktowanej HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

`For Each` Słowie kluczowym występuje zmienna, która reprezentuje jeden element w kolekcji (w tym przykładzie `myItem`), a następnie `In` — słowo kluczowe, a następnie według kolekcji ma pętli. W treści `For Each` pętli, są dostępne bieżącego elementu przy użyciu zgłoszonego wcześniej zmiennej.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Aby utworzyć bardziej ogólnego przeznaczenia pętli, użyj `Do While` instrukcji:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Ta pętla rozpoczyna się od `Do While` — słowo kluczowe, a następnie wybierz warunek, a następnie bloku powtórzeń. Zwykle zwiększenie pętli (dodać) lub zmniejszenie (odejmowanie) zmiennej lub obiekt używany do zliczania. W tym przykładzie `+=` operator dodaje 1 wartość zmiennej z każdym uruchomieniu pętli. (Aby zmniejszyć zmiennej w pętli, które zlicza w dół, użyje operator dekrementacji `-=`.)

## <a name="objects-and-collections"></a>Obiektów i kolekcji

Prawie wszystko w witrynie sieci Web platformy ASP.NET jest obiektem, w tym strona sieci web. W tej sekcji omówiono niektóre obiekty ważne, często będą korzystać z kodu użytkownika.

### <a name="page-objects"></a>Obiekty strony

Najbardziej podstawowa obiektu w programie ASP.NET jest to strona. Można uzyskać dostępu do właściwości obiektu strony bezpośrednio, bez żadnych obiektów kwalifikujących. Poniższy kod pobiera ścieżkę do pliku, przy użyciu `Request` obiekt strony:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Można użyć właściwości `Page` obiekt, aby pobrać wiele informacji, takich jak:

- `Request`. Jak już przeczytane, to zbiór informacji o bieżącym żądaniu, w tym, jakiego rodzaju przeglądarki zgłosił żądanie, adres URL strony tożsamości użytkownika, itp.
- `Response`. To jest zbiorem informacji na temat odpowiedzi (strona), który będzie wysyłany do przeglądarki, jeśli kod serwera zakończył działanie. Na przykład służy tej właściwości można zapisać informacji do odpowiedzi.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Kolekcja obiektów (tablice i słowników)

Kolekcja jest grupy obiektów tego samego typu, takich jak kolekcja `Customer` obiektów z bazy danych. Program ASP.NET zawiera wiele kolekcji wbudowanych, tak samo, jak `Request.Files` kolekcji.

Często będzie współpracować danych w kolekcjach. Istnieją dwa typy kolekcji wspólnej *tablicy* i *słownika*. Tablica jest przydatne, gdy chcesz przechowywanie kolekcji podobnych elementów, ale nie chcesz utworzyć oddzielne zmienną do przechowywania każdego elementu:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Dla tablic, należy zadeklarować określonego typu danych, takich jak `String`, `Integer`, lub `DateTime`. Aby wskazać, że zmienna może zawierać tablicę, Dodaj nawiasy do nazwy zmiennej w deklaracji (takie jak `Dim myVar() As String`). Można uzyskać dostępu do elementów w tablicy przy użyciu ich pozycji (indeks) lub za pomocą `For Each` instrukcji. Indeksy tablicy są liczony od zera &#8212; oznacza to, że pierwszy element jest w pozycji 0, drugi element znajduje się na pozycji 1 i tak dalej.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Można określić liczbę elementów w tablicy, pobierając jego `Length` właściwości. Aby uzyskać położenie konkretny element w tablicy (oznacza to, aby wyszukać), użyj `Array.IndexOf` — metoda. Użytkownik może również wykonywać następujące czynności wstecznego zawartości tablicy ( `Array.Reverse` — metoda) i sortować zawartość ( `Array.Sort` metody).

Dane wyjściowe kod tablicy ciąg wyświetlany w przeglądarce:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Słownik jest kolekcją par klucz/wartość, gdzie Podaj klucz (lub nazwę) można ustawić lub pobrać odpowiadającej jej wartości:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Aby utworzyć słownika, należy użyć `New` — słowo kluczowe, aby wskazać, czy tworzysz nową `Dictionary` obiektu. Słownik można przypisać do zmiennej przy użyciu `Dim` — słowo kluczowe. Wskazuje typy danych elementów w słowniku za pomocą nawiasów ( `( )` ). Na końcu deklaracji należy dodać kolejną parę nawiasów, ponieważ jest to rzeczywiście metodę, która tworzy nowy słownik.

Aby dodać elementy do słownika, należy wywołać `Add` metody na zmienną słownika (`myScores` w takim przypadku), a następnie określ klucz i wartość. Alternatywnie można Użyj nawiasów do wskazywania klucz i czy przypisanie proste, jak w poniższym przykładzie:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Aby uzyskać wartość ze słownika, należy określić klucz w nawiasach:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Wywoływanie metody z parametrami

Jak przedstawiono wcześniej w tym artykule, obiekty, które program z ma metody. Na przykład `Database` obiekt może mieć `Database.Connect` metody. Wiele metod również mieć co najmniej jeden parametr. A *parametru* jest wartością, który jest przekazywany do metody Aby włączyć metodę zakończy się. Na przykład przyjrzeć się deklaracji `Request.MapPath` metodę, która przyjmuje trzy parametry:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Ta metoda zwraca ścieżkę fizyczną na serwerze, który odpowiada do określonej ścieżki wirtualnej. Są trzy parametry dla metody `virtualPath`, `baseVirtualDir`, i `allowCrossAppMapping`. (Zwróć uwagę, że w deklaracji, parametry są wyświetlane z typami danych dane, które akceptują będzie). Gdy ta metoda jest wywoływana, należy podać wartości parametrów wszystkich trzech.

Podczas korzystania z języka Visual Basic z użyciem składni Razor, masz dwie opcje przekazywanie parametrów do metody: *parametrów pozycyjnych* lub *parametrów nazwanych*. Aby wywołać metodę przy użyciu parametrów pozycyjnych, parametry przekazywane w kolejności strict, który jest określony w deklaracji metody. (Będzie zazwyczaj znasz to zamówienie przeczytaj dokumentację metody.) Wykonaj kolejności i nie można pominąć wszystkie parametry &#8212; Jeśli to konieczne, możesz przekazać pusty ciąg (`""`) lub wartość null dla parametrów pozycyjnych, które nie mają wartości.

W poniższym przykładzie założono istnieje folder o nazwie *skryptów* w witrynie sieci Web. Kod wywołuje `Request.MapPath` — metoda i przekazuje wartości parametrów trzy we właściwej kolejności. Wyświetla zamapowanej ścieżki wynikowej.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Gdy istnieje wiele parametrów dla metody, można zachować swój kod czyszczący i bardziej czytelny przy użyciu parametrów nazwanych. Aby wywołać metodę przy użyciu nazwane parametry, określ nazwę parametru, a następnie `:=` , a następnie wprowadź wartość. Zaletą nazwane parametry jest, że możesz dodać je w dowolnej kolejności, która ma. (Wadą jest wywołanie metody nie jest możliwie kompaktowego).

Poniższy przykład wywołuje tę samą metodę jak powyżej, ale używa nazwanych parametrów umożliwiają określanie wartości:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Jak widać, parametry są przekazywane w innej kolejności. Jednak po uruchomieniu poprzedni przykład, jak i w tym przykładzie zostanie zwrócona taką samą wartość.

## <a name="handling-errors"></a>Obsługa błędów

### <a name="try-catch-statements"></a>Instrukcje try-Catch

Konieczne będzie często instrukcje w kodzie, który może zakończyć się niepowodzeniem ze względów poza formantu. Na przykład:

- Jeśli kod spróbuje otworzyć, utworzyć, odczytać lub zapisać plik, wszystkie rodzaje błędów mogą wystąpić. Żądany plik nie istnieje, może być zablokowane, kod może nie mieć uprawnień i tak dalej.
- Podobnie jeśli kod spróbuje do aktualizowania rekordów w bazie danych, mogą być problemy z uprawnieniami, mogą być opuszczane połączenia z bazą danych, można zapisać danych może być nieprawidłowa i tak dalej.

W terminologii programistycznej tych sytuacji są nazywane *wyjątki*. Jeśli kod napotkał wyjątek, generuje (zgłasza) komunikatu o błędzie oznacza to, co najlepiej irytujących dla użytkowników.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

W sytuacjach, gdy kod może wystąpić wyjątki, a w celu uniknięcia komunikaty o błędach tego typu, można użyć `Try/Catch` instrukcje. W `Try` instrukcji, uruchom kod, który jest sprawdzanie. W co najmniej jednej `Catch` instrukcji, można wyszukać określonych błędów (określonych typów wyjątków), które mogły wystąpić. Może zawierać tyle `Catch` instrukcje jako użytkownik musiał błędów, które są przewidywania.

> [!NOTE]
> Zaleca się unikać używania `Response.Redirect` metoda `Try/Catch` instrukcji, ponieważ może spowodować wyjątek na stronie.


W poniższym przykładzie przedstawiono strona, która tworzy plik tekstowy na pierwsze żądanie, a następnie wyświetla przycisku, który umożliwia użytkownikowi otwieranie pliku. W przykładzie użyto celowo Nieprawidłowa nazwa pliku, aby go spowoduje, że wystąpił wyjątek. Kod zawiera `Catch` instrukcji na dwa wyjątki to możliwe: `FileNotFoundException`, który występuje, jeśli nazwa pliku jest nieprawidłowa, i `DirectoryNotFoundException`, które występuje, gdy ASP.NET nawet nie można odnaleźć folderu. (Użytkownik może usuń znaczniki komentarza do instrukcji w tym przykładzie aby zobaczyć, jak jest uruchamiana po wszystko działa prawidłowo.)

Jeśli kod nie obsłużyć wyjątek, zobaczysz stronę błędu, takie jak poprzednie zrzut ekranu. Jednak `Try/Catch` sekcja pomoże uniemożliwienia użytkownika tego typu błędów.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Dodatkowe zasoby

### <a name="reference-documentation"></a>Dokumentacji

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Język Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
