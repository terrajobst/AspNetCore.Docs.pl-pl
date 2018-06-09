---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Dodawanie widoku (VB) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: c9675eb7776116ecbe910d5515abfe9b4391df22
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873355"
---
<a name="adding-a-view-vb"></a>Dodawanie widoku (VB)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodem źródłowym VB.NET jest dostępna powiązany z tym tematem. [Pobierz wersję VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz C#, przełącz się do [wersji języka C#](../cs/adding-a-view.md) tego samouczka.


W tej sekcji zamierzamy zmodyfikować `HelloWorldController` klasę, aby użyć widoku pliku szablonu do testu Hermetyzowanie proces generowania odpowiedzi HTML do klienta.

Zacznijmy od przy użyciu szablonu widoku z `Index` metoda `HelloWorldController` klasy. Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony należące do klasy kontrolera. Zmień `Index` metodę, aby zwrócić `View` obiektów, jak pokazano poniżej:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Teraz Dodajmy szablonu widoku do naszej projektu, które firma Microsoft może wywołać z `Index` metody. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz `Index` — metoda i kliknij przycisk **Dodaj widok**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**Dodaj widok** zostanie wyświetlone okno dialogowe. Pozostaw domyślnych wpisów, a następnie kliknij przycisk **Dodaj** przycisku.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld* folderu i *MvcMovie\Views\HelloWorld\Index.vbhtml* plików są tworzone. Można wyświetlić je w **Eksploratora rozwiązań**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Dodaj kod HTML pod `<h2>` tagu. Zmodyfikowane *MvcMovie\Views\HelloWorld\Index.vbhtml* plików są wyświetlane poniżej.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Uruchom aplikację i przejdź do &quot;Witaj świecie&quot; kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie wykonać dużo pracy; po prostu uruchomił instrukcję `return View()`, który wskazano, możemy użyć pliku szablonu widoku do renderowania odpowiedzi do klienta. Ponieważ firma Microsoft nie jawnie określono nazwę pliku szablonu widok do użycia, ASP.NET MVC używa domyślnie *Index.vbhtml* widoku pliku w *\Views\HelloWorld* folderu. Na poniższym obrazie pokazano ciąg ustalony w widoku.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Wygląda bardzo dobre. Jednak należy zauważyć, że pasek tytułu w przeglądarce mówi &quot;indeksu&quot; i Duży tytuł na stronie mówi &quot;Moja aplikacja MVC.&quot; Zmieńmy te.

## <a name="changing-views-and-layout-pages"></a>Zmiana widoków i układ strony

Po pierwsze Zmieńmy tekst &quot;Moja aplikacja MVC.&quot; Ten tekst jest udostępniana i pojawia się na każdej stronie. Faktycznie pojawia się tylko w jednym miejscu w naszym projektu, mimo że znajduje się na każdej stronie w naszej aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.vbhtml* pliku. Ten plik jest nazywany strony układu i jest udostępniony &quot;powłoki&quot; korzystające z innych stron.

Uwaga `@RenderBody()` wiersza kodu w dolnej części pliku. `RenderBody` jest to symbol zastępczy, gdzie wszystkie strony tworzenia widoczne, &quot;opakowana&quot; na stronie układu. Zmień `<h1>` nagłówek z **&quot;** Moja aplikacja MVC&quot; do &quot;MVC Movie App&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Uruchom aplikację i zanotuj, teraz mówi &quot;MVC Movie App&quot;. Kliknij przycisk **o** łącza, a strona przedstawia &quot;MVC Movie App&quot;, zbyt.

Pełną  *\_Layout.vbhtml* pliku przedstawiono poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teraz Zmieńmy tytuł strony indeksu (Widok).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Otwórz *MvcMovie\Views\HelloWorld\Index.vbhtml*. Istnieją dwa miejsca do dokonania zmiany: najpierw tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Wybierzemy ich nieco inne pozwala zobaczyć, które fragmentem kodu zmienia której części aplikacji.

Uruchom aplikację i przejdź do`http://localhost:xx/HelloWorld`. Należy zauważyć, że tytuł przeglądarki, nagłówek głównej i dodatkowej nagłówki zostały zmienione. To proste wprowadzić zmiany duży do aplikacji z niewielkich zmian w widoku. (Jeśli nie widzisz zmian w przeglądarce, użytkownik może mieć możliwość wyświetlania zawartości w pamięci podręcznej. Naciśnij klawisz Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nasze niewielki &quot;danych&quot; (w tym przypadku &quot;Hello World!&quot; wiadomości) jest ustalony, mimo że. Naszej aplikacji MVC ma V (widoki) i mamy C (kontrolery), ale nie M (model) jeszcze. Wkrótce, zostanie omówiony sposób utworzyć bazę danych i pobrać modelu danych.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Przed możemy przejdź do bazy danych i porozmawiać na temat modeli, umożliwia najpierw porozmawiać na temat przekazywania informacji z kontrolera do widoku. Chcemy przekazać szablonu widoku wymaga do renderowania odpowiedzi HTML do klienta. Te obiekty są zazwyczaj tworzone i przekazany przez klasę kontrolera do szablonu widoku i ich powinna zawierać tylko dane, które wymaga szablon widoku — i nie więcej.

Wcześniej z `HelloWorldController` klasy `Welcome` trwało metody akcji `name` i `numTimes` parametr, a następnie dane wyjściowe wartości parametrów w przeglądarce. Zamiast niż kontroler renderowania odpowiedź bezpośrednio w dalszym ciągu, zamiast tego firma Microsoft będzie umieśćmy tych danych w torebce widoku. Kontrolery i widoki mogą używać `ViewBag` obiektu do przechowywania danych. Który zostanie przekazany szablon widoku automatycznie i używany do renderowania odpowiedzi HTML przy użyciu zawartość zbiorze jako dane. W ten sposób kontroler dotyczy rzecz i Wyświetl szablon z inną, co pozwala na obsługę czystą &quot;separacji&quot; w aplikacji.

Alternatywnie firma Microsoft może definiowania klasy niestandardowej, Utwórz wystąpienie obiektu na własną, wypełnić go danymi i przekaż go do widoku. Często jest to ViewModel, ponieważ jest niestandardowy Model widoku. W przypadku małej ilości danych jednak obiekt ViewBag rozwiązanie idealne.

Wróć do *HelloWorldController.vb* zmianę pliku `Welcome` metody w ramach kontrolera w celu uruchomienia komunikat i NumTimes obiekt ViewBag. Obiekt ViewBag jest to obiekt dynamiczny. Oznacza to, że można umieścić niezależnie od mają być w nim. Obiekt ViewBag nie ma zdefiniowanej właściwości dopóki coś wewnątrz put.

Pełną `HelloWorldController.vb` z nową klasę w tym samym pliku.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Teraz naszych obiekt ViewBag zawiera dane, które zostaną przekazane za pośrednictwem widoku automatycznie. Ponownie można również firma Microsoft może przeszły w obiekcie własne takie jeśli mamy zbędne:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Teraz potrzebujemy `WelcomeView` szablonu! Uruchom aplikację, więc nowy kod ma być kompilowana. Zamknij przeglądarkę, kliknij prawym przyciskiem myszy wewnątrz `Welcome` metody, a następnie kliknij przycisk **Dodaj widok**.

Oto, co Twoje **Dodaj widok** wygląda okno dialogowe.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Dodaj następujący kod pod `<h2>` elementu w nowym <em>Zapraszamy.</em> Plik vbhtml. Firma Microsoft będzie pętlę wprowadzić i wypowiedz &quot;Hello&quot; tyle razy, ile użytkownik odpowie możemy powinien!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Uruchom aplikację i przejdź do `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Obecnie dane są pobierane z adresu URL i automatycznie przekazywane do kontrolera. Kontroler pakietów zapasową danych do `Model` obiekt i przekazuje, które obiekt do widoku. Widok nie wyświetla dane w postaci kodu HTML dla użytkownika.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Źródło, które były rodzaju z &quot;M&quot; dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy samouczka jest i utworzyć bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)
