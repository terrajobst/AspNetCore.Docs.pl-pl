---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Dodawanie widoku (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-c"></a>Dodawanie widoku (C#)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej. Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)
> 
> Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer z kodu źródłowego C# jest dostępna powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz Visual Basic, przełącz się do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.


W tej sekcji zamierzasz zmodyfikować `HelloWorldController` klasę, aby użyć widoku pliki szablonu, aby bezpośrednio Hermetyzowanie proces generowania odpowiedzi HTML do klienta.

Zostaną utworzone przy użyciu nowego pliku szablonu widoku [aparatu widoku Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) wprowadzonego w programie ASP.NET MVC 3. Szablony razor na podstawie widoku mają *.cshtml* rozszerzenie pliku i zapewnić atrakcyjny sposób utworzenia kodu HTML w danych wyjściowych przy użyciu języka C#. Razor minimalizuje liczbę znaków i wymagane podczas zapisywania szablonu widoku naciśnięć klawiszy i umożliwia szybkie, płynu kodowania przepływu pracy.

Uruchom przy użyciu szablonu widoku z `Index` metoda `HelloWorldController` klasy. Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony klasy kontrolera. Zmień `Index` metodę, aby zwrócić `View` obiektów, jak pokazano poniżej:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Ten kod szablonu widoku są używane do generowania odpowiedzi HTML do przeglądarki. W projekcie Dodaj szablon widoku, który można używać z `Index` metody. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz `Index` — metoda i kliknij przycisk **Dodaj widok**.

![](adding-a-view/_static/image1.png)

**Dodaj widok** zostanie wyświetlone okno dialogowe. Pozostaw wartości domyślne są a kliknij przycisk **Dodaj** przycisk:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* folderu i *MvcMovie\Views\HelloWorld\Index.cshtml* plików są tworzone. Można wyświetlić je w **Eksploratora rozwiązań**:

![](adding-a-view/_static/image3.png)

Poniżej przedstawiono *Index.cshtml* pliku, który został utworzony:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Dodaj kod HTML pod `<h2>` tagu. Zmodyfikowane *MvcMovie\Views\HelloWorld\Index.cshtml* plików są wyświetlane poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Uruchom aplikację i przejdź do `HelloWorld` kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie wykonać dużo pracy; po prostu uruchomił instrukcję `return View()`, który określony metody należy użyć pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie został jawnie określić nazwę pliku szablonu widok do użycia, ASP.NET MVC używa domyślnie *Index.cshtml* pliku widoku w *\Views\HelloWorld* folderu. Na poniższym obrazie pokazano ciąg ustalony w widoku.

![](adding-a-view/_static/image6.png)

Wygląda bardzo dobre. Jednak zauważyć, że pasek tytułu w przeglądarce mówi, "Index", a duży tytuł na stronie mówi "Moja aplikacja MVC." Zmieńmy te.

## <a name="changing-views-and-layout-pages"></a>Zmiana widoków i układ strony

Po pierwsze chcesz zmienić tytuł "Moja aplikacja MVC" w górnej części strony. Ten tekst jest wspólny dla każdej strony. Faktycznie jest stosowana w tylko jednym miejscu w projekcie, nawet jeśli wygląda na to, na wszystkich stronach w aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.cshtml* pliku. Ten plik jest nazywany *układ strony* i jest udostępniony "powłoka" używanego w innych stron.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Szablony układu umożliwiają określ układ kontenera HTML witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie sieci. Uwaga `@RenderBody()` wiersza w dolnej części pliku. `RenderBody` jest to symbol zastępczy, gdzie wszystkich stron tworzonych w specyficznych dla widoku wyświetlane, "zawinięty", strony układu. Zmiana pozycji tytułu w szablon układu z "Moja aplikacja MVC" do "MVC Movie App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Uruchom aplikację i zwróć uwagę, że teraz wyświetlany jest tekst "MVC Movie App". Kliknij przycisk **o** łącza i zostanie wyświetlony, jak ta strona zawiera "MVC Movie App" zbyt. Udało się zmienić raz w szablonie układ i mieć wszystkich stron w witrynie uwzględniać nowy tytuł.

![](adding-a-view/_static/image9.png)

Pełną  *\_Layout.cshtml* pliku przedstawiono poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teraz Zmieńmy tytuł strony indeksu (Widok).

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca do dokonania zmiany: najpierw tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Należy podjąć ich nieco inne pozwala zobaczyć, które fragmentem kodu zmienia której części aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Aby wskazać tytuł HTML, aby wyświetlić kod nad zestawy `Title` właściwość `ViewBag` obiektu (w *Index.cshtml* wyświetlanie szablonu). Jeśli przyjrzymy się ponownie kod źródłowy szablon układu, można zauważyć, że szablon używa tej wartości w `<title>` element jako część `<head>` sekcji kodu HTML. W ten sposób można łatwo przekazywania innych parametrów między widok szablonu i pliku układu.

Uruchom aplikację i przejdź do `http://localhost:xx/HelloWorld`. Należy zauważyć, że tytuł przeglądarki, nagłówek głównej i dodatkowej nagłówki zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, użytkownik może mieć możliwość wyświetlania zawartości w pamięci podręcznej. Naciśnij klawisz Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.)

Należy również zauważyć, jak zawartości w *Index.cshtml* Wyświetl szablon został scalony z  *\_Layout.cshtml* szablon widoku i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Szablony układu ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji.

![](adding-a-view/_static/image10.png)

Nasze niewielki "data" (w tym przypadku "Hello z naszych szablonu widoku!" komunikat) jest ustalony, mimo że. Aplikacji MVC ma "V" (Widok) i masz "C" (kontroler), ale nie "M" (model) jeszcze. Wkrótce, zostanie omówiony sposób utworzyć bazę danych i pobrać modelu danych.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Przed możemy przejdź do bazy danych i porozmawiać na temat modeli, umożliwia najpierw porozmawiać na temat przekazywania informacji z kontrolera do widoku. Klasy kontrolera są wywoływane w odpowiedzi na żądania przychodzącego adresu URL. Klasa kontrolera jest, gdzie napisać kod, który obsługuje przychodzące parametry, pobiera dane z bazy danych i ostatecznie decyduje o tym, jakiego rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetl szablony mogą następnie używane z kontrolera do generowania i format odpowiedzi HTML do przeglądarki.

Kontrolery spoczywa odpowiedzialność za zapewnienie, niezależnie od danych lub obiekty są wymagane dla szablonu widoku do renderowania odpowiedzi do przeglądarki. Szablon widoku nigdy nie należy przeprowadzić logiki biznesowej lub bezpośredniej interakcji z bazą danych. Zamiast tego powinny działać tylko z danymi, które jest podany przez kontroler. Obsługa tego "separacji" zwiększają kod czysty i łatwiejsze do utrzymania.

Obecnie `Welcome` metody akcji w `HelloWorldController` klasy przyjmuje `name` i `numTimes` parametr, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast renderowania tej odpowiedzi jako ciąg kontrolera, umożliwia zmianę kontrolera zamiast tego użyć szablonu widoku. Szablon widoku wygeneruje odpowiedzi dynamicznych, co oznacza, że trzeba przekazać odpowiednich bitów danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić przez kontroler umieszczanie danych dynamicznych, które wymaga Wyświetl szablon o `ViewBag` obiekt, który szablon widoku można następnie uzyskać dostęp.

Wróć do *HelloWorldController.cs* plików i zmień `Welcome` metody w celu dodania `Message` i `NumTimes` do wartości `ViewBag` obiektu. `ViewBag` jest to obiekt dynamiczny, co oznacza, że możesz umieścić dowolne `ViewBag` obiekt nie ma zdefiniowanej właściwości dopóki coś wewnątrz put. Pełną *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Teraz `ViewBag` obiekt zawiera dane, które będą automatycznie przekazywane do widoku.

Następnie należy szablonu widoku Zapraszamy! W **debugowania** menu, wybierz opcję **kompilacji MvcMovie** się upewnić, że projekt jest kompilowany.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Kliknij prawym przyciskiem myszy wewnątrz `Welcome` — metoda i kliknij przycisk **Dodaj widok**. Oto, co **Dodaj widok** okno dialogowe wygląda następująco:

![](adding-a-view/_static/image13.png)

Kliknij przycisk **Dodaj**, a następnie dodaj następujący kod w obszarze `<h2>` elementu w nowym *Welcome.cshtml* pliku. Utworzysz pętli, które mówi "tekst Hello" dowolną liczbę razy, ile użytkownik wyświetla informację, że powinien on. Pełną *Welcome.cshtml* plików są wyświetlane poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Uruchom aplikację, a następnie przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Obecnie dane są pobierane z adresu URL i automatycznie przekazywane do kontrolera. Kontroler pakiety danych w `ViewBag` obiekt i przekazuje, które obiekt do widoku. Widok następnie wyświetla dane jako kodu HTML dla użytkownika.

![](adding-a-view/_static/image14.png)

Dobrze, która jest typu "M" dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy samouczka jest i utworzyć bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)
