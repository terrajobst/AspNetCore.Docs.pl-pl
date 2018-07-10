---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Widoków ASP.NET MVC — omówienie (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Co to jest widok ASP.NET MVC i jak różni się on ze strony HTML? W tym samouczku Walther Autor: Stephen stanowi wprowadzenie do widoków i pokazuje, jak można t...'
ms.author: aspnetcontent
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: d2fc96f7e991dd7c4e0b3e9ff5c589c1075010ac
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833662"
---
<a name="aspnet-mvc-views-overview-c"></a>Widoków ASP.NET MVC — omówienie (C#)
====================
przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Co to jest widok ASP.NET MVC i jak różni się on ze strony HTML? W tym samouczku Walther Autor: Stephen stanowi wprowadzenie do widoków i pokazuje, jak możesz korzystać z zalet danych widoku i pomocników HTML w widoku.


Celem tego samouczka jest zapewnienie krótkie wprowadzenie do widoków ASP.NET MVC, wyświetlanie danych i pomocników HTML. Do końca tego samouczka należy wiedzieć, jak tworzenie nowych widoków, przekazać dane z kontrolera do widoku i użyć pomocników HTML do generowania zawartości w widoku.

## <a name="understanding-views"></a>Objaśnienie widoków

Dla platformy ASP.NET lub Active Server Pages platformy ASP.NET MVC nie obejmuje wszystko, co odnosi się bezpośrednio do strony. W aplikacji ASP.NET MVC nie istnieje strona na dysku, który odnosi się do ścieżki na adres URL, który można wpisać w pasku adresu przeglądarki. Najbliższy rzeczą do strony w aplikacji ASP.NET MVC jest coś, co o nazwie *widoku*.

ASP.NET MVC, w przeglądarce aplikacji, przychodzące żądania są mapowane do akcji kontrolera. Akcja kontrolera może zwrócić widok. Akcja kontrolera może jednak wykonać inny rodzaj akcje, takie jak przekierowywanie do kolejnej akcji kontrolera.

Wyświetlanie listy 1 zawiera proste o nazwie HomeController kontrolera. HomeController udostępnia dwie akcje kontroler o nazwie indeks() i Details().

**Wyświetlanie listy 1 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Możesz wywołać pierwszą akcję działania indeks(), wpisując następujący adres URL w pasku adresu przeglądarki:

/ Home/Index

Możesz wywołać drugiej akcji, akcja Details(), wpisując ten adres w przeglądarce:

/ Home/Details

Akcja indeks() zwraca widok. Większość czynności, które tworzysz zwróci widoków. Jednak działanie może zwrócić inne typy wyników akcji. Na przykład akcja Details() zwraca RedirectToActionResult, który przekierowuje żądania przychodzące do działania indeks().

Akcja indeks() zawiera następujące jednego wiersza kodu:

View();

Ten wiersz kodu zwraca widok, który musi znajdować się w następującej ścieżce na serwerze sieci web:

\Views\Home\Index.aspx

Ścieżka widoku wynika z nazwy kontrolera i nazwy akcji kontrolera.

Jeśli wolisz, może być wyraźnie widoku. Następujący wiersz kodu zwraca widok o nazwie Fred:

Widok (Fred);

Po wykonaniu ten wiersz kodu widok jest zwracana z następującą ścieżkę:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Jeśli planujesz utworzenie testów jednostkowych dla aplikacji ASP.NET MVC jest dobry pomysł, aby była niejawna przy korzystaniu nazwy widoku. W ten sposób można utworzyć test jednostkowy, aby sprawdzić, czy oczekiwany widok został zwrócony przez akcji kontrolera.


## <a name="adding-content-to-a-view"></a>Dodawanie zawartości do widoku

Widok jest standardowego (dokumentu HTML, który może zawierać skryptów X). Skrypty umożliwia dodawanie zawartości dynamicznej do widoku.

Na przykład widok w ofercie 2 Wyświetla bieżącą datę i godzinę.

**Wyświetlanie listy 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Zwróć uwagę, że treść strony HTML znajdującej się w ofercie 2 zawiera następujący skrypt:

&lt;% Response.Write(DateTime.Now);%&gt;

Możesz użyć ograniczników skryptu &lt;% i %&gt; do oznaczania początku i końcu skryptu. Ten skrypt został napisany w języku C#. Wyświetla bieżącą datę i godzinę, wywołując metodę Response.Write() do renderowania zawartości do przeglądarki. Ograniczniki skryptu &lt;% i %&gt; można wykonać jedną lub więcej instrukcji.

Ponieważ wywołujesz Response.Write() tak często, firma Microsoft umożliwia skrót do wywoływania metody Response.Write(). Wyświetl w ofercie 3 korzysta z ogranicznikami &lt;% = i %&gt; jako skrót do wywoływania Response.Write().

**Wyświetlanie listy 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Można użyć dowolnego języka platformy .NET do generowania zawartości dynamicznej w widoku. Zwykle ll możesz użyć Visual Basic .NET lub C# do pisania widoków i kontrolerów.

## <a name="using-html-helpers-to-generate-view-content"></a>Wyświetlanie zawartości przy użyciu pomocników HTML

Aby ułatwić dodawanie zawartości do widoku, możesz korzystać z zalet coś, co jest nazywane *pomocnika kodu HTML*. Pomocnik kodu HTML, zazwyczaj jest metodą, która generuje ciąg. Pomocników HTML można użyć do wygenerowania standardowych elementów HTML, takich jak pola tekstowe, linki, listy rozwijane i pola listy.

Na przykład widok w ofercie 4 wykorzystuje trzy pomocników HTML — pomocnicy BeginForm() TextBox() i Password() — do generowania nazwy logowania formularza (patrz rysunek 1).

**Wyświetlanie listy 4 – \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![Okno dialogowe Nowy projekt](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Rysunek 01**: standardowy formularz logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-views-overview-cs/_static/image2.png))


Wszystkie metody pomocników HTML są nazywane we właściwości Html widoku. Na przykład przez wywołanie metody Html.TextBox() renderowanie pole tekstowe.

Zwróć uwagę, że używasz ograniczników skryptu &lt;% = i %&gt; podczas wywoływania Html.TextBox() i Html.Password() pomocników. Pomocnicy te po prostu zwrócenia ciągu. Należy wywołać Response.Write() do wyświetlenia ciągu do przeglądarki.

Użycie metody pomocnika kodu HTML jest opcjonalne. One ułatwiać pracę dzięki zmniejszeniu ilości kodu HTML i skryptu, który należy napisać. Wyświetl listę 5 renderuje dokładnie tego samego formularza jako widok w ofercie 4 bez użycia pomocników HTML.

**Wyświetlanie listy 5 — \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

Istnieje również możliwość tworzenia własnych pomocników HTML. Na przykład można utworzyć metody pomocnika GridView(), który automatycznie wyświetla zestaw rekordów bazy danych w tabeli HTML. W tym samouczku dowiesz się w tym temacie **Tworzenie niestandardowych pomocników HTML**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Za pomocą widoku danych, aby przekazać dane do widoku

Wyświetlanie danych służy do przekazywania danych za pomocą kontrolera do widoku. Traktuj dane widoku, takie jak pakiet wysyłanych za pośrednictwem wiadomości e-mail. Wszystkie dane przekazane za pomocą kontrolera do widoku musi zostać wysłany za pomocą tego pakietu. Na przykład kontroler w ofercie 6 dodaje komunikat do wyświetlania danych.

**Wyświetlanie listy 6 - ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

Kontroler ViewData właściwość reprezentuje kolekcję par nazw i wartości. W ofercie 6 metoda indeks() dodaje element do widoku zbierania danych o nazwie message wartością Hello World!. Gdy widok jest zwracany przez metodę indeks(), dane widoku są automatycznie przekazywane do widoku.

Wyświetl w ofercie 7 pobiera komunikat z danymi widoku i renderuje komunikat do przeglądarki.

**Wyświetlanie listy 7 – \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Należy zauważyć, że widok wykorzystuje metody pomocnika kodu HTML Html.Encode() podczas renderowania komunikatu. Pomocnik kodu HTML Html.Encode() koduje znaki specjalne, takie jak &lt; i &gt; na znaki, które są bezpieczne wyświetlić na stronie sieci web. Zawsze, gdy renderowanie zawartości, który użytkownik prześle do witryny sieci Web należy zakodować zawartość, aby uniemożliwić ataki przez iniekcję kodu JavaScript.

(Ponieważ utworzyliśmy komunikat osoby w ProductController, firma Microsoft don t naprawdę potrzebne do zakodowania komunikatu. Jednak jest dobry sposób zawsze wywołuj metody Html.Encode() podczas wyświetlania zawartości pobranej z wyświetlanie danych w widoku).

W ofercie 7 Firma Microsoft skorzystała z widoku danych komunikatu prosty ciąg znaków za pomocą kontrolera do widoku. Również służy widok danych do przekazania innych typów danych, takich jak zbiór rekordów bazy danych, za pomocą kontrolera do widoku. Na przykład jeśli chcesz wyświetlić zawartość tabeli Produkty bazy danych w widoku, a następnie przejdzie kolekcji bazy danych rejestruje w widoku danych.

Istnieje również opcja przekazywania silnie typizowanego widoku danych za pomocą kontrolera do widoku. W tym samouczku dowiesz się w tym temacie **widoków i zrozumienia silnie Typizowane dane widoku**.

## <a name="summary"></a>Podsumowanie

W tym samouczku pod warunkiem krótkie wprowadzenie do widoków ASP.NET MVC, wyświetlanie danych i pomocników HTML. W pierwszej sekcji przedstawiono sposób dodawania nowych widoków do projektu. Wiesz, że należy dodać widok prawidłowego folderu w celu wywoływania go z określonego kontrolera. Następnie omówiono temat pomocników HTML. Pokazaliśmy ci, jak pomocników HTML umożliwiają łatwe generowanie standardowych zawartość HTML. Na koniec pokazaliśmy ci, jak korzystać z zalet danych widoku, aby przekazać dane z kontrolera do widoku.

> [!div class="step-by-step"]
> [Next](creating-custom-html-helpers-cs.md)
