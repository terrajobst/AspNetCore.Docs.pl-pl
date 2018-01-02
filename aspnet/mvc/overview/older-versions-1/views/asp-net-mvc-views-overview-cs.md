---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: "ASP.NET MVC widoków — omówienie (C#) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Co to jest widok ASP.NET MVC i jak go różni się od strony HTML? W tym samouczku Stephen Walther stanowi wprowadzenie do widoków i pokazuje, jak można t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de095b0621af3b6166a2e1cbcb1c63c26a88aa2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-views-overview-c"></a>ASP.NET MVC widoków — omówienie (C#)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> Co to jest widok ASP.NET MVC i jak go różni się od strony HTML? W tym samouczku Stephen Walther stanowi wprowadzenie do widoków i pokazuje, jak można korzystać z danych widoku i pomocników HTML w widoku.


Celem tego samouczka jest dostarczają krótkie wprowadzenie do platformy ASP.NET MVC widoków danych widoku i pomocników HTML. Na koniec tego samouczka należy wiedzieć, jak utworzyć nowe widoki, przekazać dane z kontrolera do widoku i używać pomocników HTML do generowania zawartości w widoku.

## <a name="understanding-views"></a>Opis widoków

Dla programu ASP.NET lub Active Server Pages platformy ASP.NET MVC nie zawiera wszystko, co odpowiada bezpośrednio do strony. W aplikacji platformy ASP.NET MVC nie istnieje strona na dysku, który odpowiada ścieżki w adresie URL można wpisać w pasku adresu przeglądarki. Najbliższy co do strony w aplikacji platformy ASP.NET MVC to element o nazwie *widoku*.

Aplikacja przeglądarki przychodzące żądanie jest mapowane na akcji kontrolera ASP.NET MVC. Akcja kontrolera może zwrócić widok. Jednak akcji kontrolera może wykonywać innego typu akcji, takie jak przekierowywanie należy do innej akcji kontrolera.

Wyświetlanie listy 1 zawiera proste kontrolerze o nazwie HomeController. HomeController przedstawia o nazwie indeks() i Details() dwóch akcji kontrolera.

**Wyświetlanie listy 1 - HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Wprowadź następujący adres URL na pasku adresu przeglądarki, można wywołać pierwszą akcją akcji indeks():

/ Głównej/indeksu

Drugą akcję, akcja Details() można wywoływać, wpisując ten adres w przeglądarce:

/ Głównej/szczegółów

Akcja indeks() zwraca widok. Większość akcji, które możesz utworzyć zwróci widoków. Jednak akcja może zwrócić innych typów wyników akcji. Na przykład akcja Details() zwraca RedirectToActionResult, który przekierowuje żądania przychodzące do działania indeks().

Akcja indeks() zawiera jednym następującego kodu:

View();

Ten wiersz kodu zwraca widok, który musi znajdować się w następującej ścieżce na serwerze sieci web:

\Views\Home\Index.aspx

Ścieżka do widoku jest wywnioskowany na podstawie nazwy kontrolera i nazwy akcji kontrolera.

Jeśli wolisz, może być jawne dotyczące widoku. Następujący wiersz kodu zwraca widok o nazwie Krzysztof:

Widok (Krzysztof);

Po wykonaniu ten wiersz kodu widoku jest zwracana z następującej ścieżki:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Jeśli planujesz tworzenia testów jednostkowych dla aplikacji ASP.NET MVC istnieje dobrze jawne o nazwy widoku. W ten sposób można utworzyć testu jednostkowego, aby sprawdzić widok oczekiwanego zwróciło akcji kontrolera.


## <a name="adding-content-to-a-view"></a>Dodawanie zawartości do widoku

Widok jest standardem (dokument HTML, który może zawierać skryptów X). Skrypty umożliwia dodanie do widoku zawartości dynamicznej.

Na przykład widok wyświetlania 2 Wyświetla bieżącą datę i godzinę.

**Wyświetlanie listy 2 - \Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Zwróć uwagę, że strony HTML w wyświetlania 2 zawiera następujący skrypt:

&lt;% Response.Write(DateTime.Now); %&gt;

Użyj ograniczników skryptu &lt;% i %&gt; aby oznaczyć początek i koniec skryptu. Ten skrypt jest napisany w języku C#. Wyświetla bieżącą datę i godzinę, wywołując metodę metody Response.Write() do renderowania zawartości w przeglądarce. Ograniczniki skryptu &lt;% i %&gt; może służyć do wykonywania instrukcji jeden lub więcej.

Ponieważ często wywołania metody Response.Write(), firma Microsoft umożliwia skrót dla wywołania metody Response.Write() metody. Widok w 3 wyświetlania używa ograniczniki &lt;% = i %&gt; jako skrót do wywołania metody Response.Write().

**Wyświetlanie listy 3 - Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Można użyć dowolnego języka .NET do generowania zawartości dynamicznej w widoku. Zwykle pojawi Użyj Visual Basic .NET lub C# do zapisu, widoków i kontrolerów.

## <a name="using-html-helpers-to-generate-view-content"></a>Wyświetl zawartość przy użyciu pomocników HTML

Aby ułatwić dodawanie zawartości do widoku, można korzystać z coś o nazwie *pomocnika kodu HTML*. Pomocnika kodu HTML, zazwyczaj jest to metoda, która generuje ciąg. Pomocników HTML służy do generowania standardowych elementów HTML, takich jak pola tekstowe, łącza, listy rozwijane i pola listy.

Na przykład widok listę 4 wykorzystuje trzy pomocników HTML — pomocników BeginForm(), TextBox() i Password() — można wygenerować identyfikatora logowania formularza (zobacz rysunek 1).

**Wyświetlanie listy 4 – \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]


[![Okno dialogowe nowego projektu](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Rysunek 01**: standardowy formularz logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-views-overview-cs/_static/image2.png))


Wszystkie metody pomocników HTML są nazywane we właściwości Html widoku. Na przykład że pole tekstowe przez wywołanie metody Html.TextBox().

Powiadomienie, że używasz ograniczników skryptu &lt;% = i %&gt; podczas wywoływania metody pomocników Html.TextBox() i Html.Password(). Te pomocników po prostu zwraca ciąg. Należy wywołać metody Response.Write() do renderowania ciąg do przeglądarki.

Przy użyciu metody pomocnika kodu HTML jest opcjonalna. One ułatwiać pracę dzięki zmniejszeniu ilości HTML i skryptu, który należy napisać. Wyświetl listę 5 renderuje dokładnie tego samego formularza jako widok na listę 4 bez użycia pomocników HTML.

**Wyświetlanie listy 5 — \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

Istnieje również możliwość utworzenia własnych pomocników HTML. Na przykład można utworzyć metody pomocnika GridView(), która automatycznie wyświetla zestaw rekordów bazy danych w tabeli HTML. W tym temacie firma Microsoft Eksplorowanie w samouczku **Tworzenie niestandardowych pomocników HTML**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Przy użyciu widoku danych do przekazywania danych do widoku

Wyświetlanie danych służy do przekazywania danych z kontrolera do widoku. Należy traktować danych widoku, takich jak pakiet, który możesz wysłać pocztą. Należy wysłać wszystkie dane przekazane z kontrolera do widoku przy użyciu tego pakietu. Na przykład kontroler na wyświetlanie 6 dodaje komunikat, aby wyświetlić dane.

**Wyświetlanie listy 6 - ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

Kontroler właściwości ViewData reprezentuje kolekcję par nazw i wartości. Wyświetlanie listy 6 metody indeks() dodaje element do widoku zbierania danych o nazwie komunikat z wartością Hello World!. Gdy widok jest zwracany przez metodę indeks(), dane widoku są automatycznie przekazywane do widoku.

Widok w wyświetlania 7 pobiera wiadomość z danymi widoku i renderuje komunikat do przeglądarki.

**Wyświetlanie listy 7 — \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Należy zauważyć, że widok wykorzystuje metody pomocnika kodu HTML Html.Encode() podczas renderowania komunikatu. Pomocnik kodu HTML Html.Encode() koduje znaki specjalne, takie jak &lt; i &gt; na znaki, które są bezpieczne wyświetlić na stronie sieci web. Zawsze, gdy renderowania zawartości, którą użytkownik prześle do witryny sieci Web, należy zakodować zawartości, aby zapobiec atakom iniekcji JavaScript.

(Ponieważ utworzone komunikat nad ProductController, możemy ADAM t naprawdę potrzebne do kodowania komunikatu. Jednak jest dobrym nawyk zawsze wywołać metody Html.Encode(), podczas wyświetlania zawartości pobranej z danych widoku w widoku).

Wyświetlanie listy 7 Wybraliśmy zaletą danych widoku do przekazywania wiadomości prostego ciągu z kontrolera do widoku. Również umożliwia wyświetlanie danych przekazywania innych typów danych, takich jak kolekcja rekordów bazy danych, z kontrolera do widoku. Na przykład jeśli chcesz wyświetlenia zawartości tabeli Produkty bazy danych w widoku, a następnie przejdzie kolekcji bazy danych rejestruje w widoku danych.

Istnieje również opcja przekazywania danych silnie typizowanego widoku z kontrolera do widoku. W tym temacie firma Microsoft Eksplorowanie w samouczku **opis silnie Typizowanej danych widoku i widoki**.

## <a name="summary"></a>Podsumowanie

W tym samouczku podać krótkie wprowadzenie do platformy ASP.NET MVC widoków danych widoku i pomocników HTML. W pierwszej sekcji przedstawiono sposób dodawania nowych widoków do projektu. Wiesz, że należy dodać widok do prawidłowego folderu celu wywołania go z danego kontrolera. Następnie Rozmawialiśmy temat pomocników HTML. Przedstawiono sposób pomocników HTML umożliwiają łatwe generowanie standardowe zawartość HTML. Ponadto przedstawiono sposób korzystać z danych widoku do przekazywania danych z kontrolera do widoku.

>[!div class="step-by-step"]
[Dalej](creating-custom-html-helpers-cs.md)
