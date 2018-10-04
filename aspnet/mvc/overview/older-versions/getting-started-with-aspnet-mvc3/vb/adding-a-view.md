---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Dodawanie widoku (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0d6311cac4fe01b2e21b300ff3841b3f2ca6fcf5
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577067"
---
<a name="adding-a-view-vb"></a>Dodawanie widoku (VB)
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-view.md) po ukończeniu tego samouczka.


W tej sekcji zamierzamy zmodyfikować `HelloWorldController` klasy przy użyciu widoku pliku szablonu, aby prawidłowo zamykaj wystąpienia hermetyzacji proces generowania odpowiedzi HTML do klienta.

Zacznijmy od przy użyciu szablonu widoku z `Index` method in Class metoda `HelloWorldController` klasy. Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony w obrębie klasy kontrolera. Zmiana `Index` metodę, aby zwrócić `View` obiektu, jak pokazano w następującym:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Teraz Dodajmy Wyświetl szablon do naszego projektu, które firma Microsoft może wywołać za pomocą `Index` metody. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz `Index` metody i kliknij przycisk **Dodaj widok**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**Dodaj widok** pojawi się okno dialogowe. Pozostaw domyślne wpisy, a następnie kliknij przycisk **Dodaj** przycisku.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld* folder i *MvcMovie\Views\HelloWorld\Index.vbhtml* plików są tworzone. Widać je w **Eksploratora rozwiązań**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Dodaj kod HTML, w obszarze `<h2>` tagu. Zmodyfikowanego *MvcMovie\Views\HelloWorld\Index.vbhtml* plików znajdują się poniżej.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Uruchom aplikację, a następnie przejdź do &quot;Witaj, świecie&quot; kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie dużo pracy; po prostu uruchomił instrukcję `return View()`, który wskazano, Chcieliśmy użyć pliku szablonu widoku do renderowania odpowiedzi do klienta. Ponieważ firma Microsoft nie jawnie określono nazwę pliku szablonu widoku, który będzie używany, ASP.NET MVC używa domyślnie *Index.vbhtml* widok pliku w ramach *\Views\HelloWorld* folderu. Na poniższym obrazie przedstawiono ciągu ustalonych w widoku.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Wygląda dość dobrze. Jednak należy zauważyć, że pasek tytułu w przeglądarce jest wyświetlany komunikat &quot;indeksu&quot; i mówi big tytuł na stronie &quot;Moja aplikacja MVC.&quot; Zmieńmy te z nich.

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i układ strony

Po pierwsze zmienimy tekst &quot;Moja aplikacja MVC.&quot; Ten tekst jest udostępniana i pojawia się na każdej stronie. Faktycznie wygląda na to w jednym miejscu w projekcie, nawet jeśli nie znajduje się na każdej stronie w naszej aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.vbhtml* pliku. Ten plik jest nazywany strony układu i jest wspólnie &quot;powłoki&quot; używanego przez innych stron.

Uwaga `@RenderBody()` wiersz kodu w dolnej części pliku. `RenderBody` jest symbolem zastępczym, gdzie wszystkie strony można tworzyć widoczne, &quot;opakowane&quot; w stronę układu. Zmiana `<h1>` nagłówek z **&quot;** Moja aplikacja MVC&quot; do &quot;aplikacja filmu MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Uruchom aplikację i zanotuj, teraz mówi &quot;aplikacja filmu MVC&quot;. Kliknij przycisk **o** łącze, a strona przedstawia &quot;aplikacja filmu MVC&quot;również.

Pełne  *\_Layout.vbhtml* plików znajdują się poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teraz Zmieńmy tytuł strony indeksu (view).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Otwórz *MvcMovie\Views\HelloWorld\Index.vbhtml*. Istnieją dwa miejsca, aby wprowadzić zmianę: po pierwsze, tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Wybierzemy je nieco, dzięki czemu można zobaczyć, które fragmentem kodu zmienia której części aplikacji.

Uruchom aplikację, a następnie przejdź do`http://localhost:xx/HelloWorld`. Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione. To można łatwo stworzyć duże zmiany w aplikacji za pomocą niewielkie zmiany do widoku. (Jeśli nie widzisz zmian w przeglądarce, prawdopodobnie przeglądasz zawartość z pamięci podręcznej. Naciśnij klawisze Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nasze trochę &quot;danych&quot; (w tym przypadku &quot;Hello World!&quot; wiadomości) jest ustalony, mimo że. Nasza aplikacja MVC ma V (widoki) i mamy C (kontrolery), ale nie M (model) jeszcze. Wkrótce, omówimy sposób tworzenia bazy danych i pobierania danych modelu z niego.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Zanim firma przejdź do bazy danych i porozmawiać na temat modeli jednak najpierw Omówmy przekazywania informacji z kontrolera do widoku. Chcemy przekazać szablonu widoku wymaga do renderowania odpowiedzi HTML do klienta. Te obiekty są zazwyczaj tworzone i przekazywane przez klasę kontrolera do widoku szablonu i powinny zawierać tylko dane, które wymaga Wyświetl szablon — i nie ma więcej.

Wcześniej przy użyciu `HelloWorldController` klasy `Welcome` metody akcji `name` i `numTimes` parametru, a następnie dane wyjściowe wartości parametrów w przeglądarce. Raczej niż kontroler renderowania odpowiedź bezpośrednio w dalszym ciągu, Przyjrzyjmy zamiast tego będzie umieściliśmy te dane w zbiorze widoku. Można użyć widoków i kontrolerów `ViewBag` obiekt do przechowywania tych danych. Który będzie być przekazywana do pliku szablonu widoku automatycznie i używany do renderowania odpowiedzi HTML przy użyciu zawartość w zbiorze danych. W ten sposób kontrolera jest rozpatrywany wraz z jedną z rzeczy i wyświetlanie szablonu z inną — dzięki czemu możemy zachować czyste &quot;separacji&quot; w aplikacji.

Alternatywnie firma Microsoft może definiowania klasy niestandardowej, a następnie utworzyć wystąpienie obiektu na własną, wypełniać danych i przekazania go do widoku. Często jest to ViewModel, ponieważ jest niestandardowy Model widoku. Małe ilości danych jednak obiekt ViewBag bezproblemowe współdziałanie.

Wróć do *HelloWorldController.vb* zmianę pliku `Welcome` metody w ramach kontrolera, aby umieścić komunikat i NumTimes w obiekt ViewBag. Obiekt ViewBag to obiekt dynamiczny. Oznacza to, że możesz umieścić dowolne nim. Obiekt ViewBag nie ma zdefiniowanej właściwości do momentu coś znajdującym się w nim umieścić.

Pełne `HelloWorldController.vb` przy użyciu nowej klasy, w tym samym pliku.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Teraz naszych elementów ViewBag zawiera dane, które zostaną przekazane za pośrednictwem widoku automatycznie. Ponownie też firma Microsoft może przeszły w własną obiekcie następująco Jeśli spodobał się nam:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Teraz należy `WelcomeView` szablonu! Uruchom aplikację, dzięki czemu nowy kod jest kompilowany. Zamknij przeglądarkę, kliknij prawym przyciskiem myszy wewnątrz `Welcome` metody, a następnie kliknij **Dodaj widok**.

Oto, co Twoja **Dodaj widok** wygląda okno dialogowe.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Dodaj następujący kod w obszarze `<h2>` elementu w nowym <em>powitalna.</em> Plik vbhtml. Firma Microsoft będzie wprowadzić pętli i wypowiedz &quot;Hello&quot; dowolną liczbę razy użytkownik odpowie, powinniśmy!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Uruchom aplikację, a następnie przejdź do `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i automatycznie przekazywane do kontrolera. Kontroler pakiety danych na `Model` obiektu i przebiegi, które obiekt widoku. Widok nie wyświetla dane w postaci kodu HTML do użytkownika.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Dobrze, który był rodzajem elementu &quot;M&quot; dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy wyjaśniono konto i bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)
