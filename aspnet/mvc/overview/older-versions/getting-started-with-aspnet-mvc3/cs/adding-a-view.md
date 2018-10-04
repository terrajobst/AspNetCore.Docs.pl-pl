---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Dodawanie widoku (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 9fc8c6cad44016511c462b4206c28ea3449ff97e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576615"
---
<a name="adding-a-view-c"></a>Dodawanie widoku (C#)
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.
> 
> 
> Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio. Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej. Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:
> 
> - [Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Program ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)
> 
> Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem. [Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.


W tej sekcji zamierzasz zmodyfikować `HelloWorldController` klasy w celu użycia widoku pliki szablonu, aby prawidłowo zamykaj wystąpienia hermetyzacji proces generowania odpowiedzi HTML do klienta.

Utworzysz widok pliku szablonu, za pomocą nowego [aparatu widoku Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) wprowadzone w programie ASP.NET MVC 3. Szablony widoku razor mają *.cshtml* rozszerzenie pliku, a następnie podaj to elegancki sposób tworzenia kodu HTML w danych wyjściowych przy użyciu języka C#. Razor minimalizuje liczbę znaków i wymagane podczas pisania szablonu widoku naciśnięcia klawiszy i umożliwia szybkie, płynów, przepływ pracy kodowania.

Uruchom przy użyciu szablonu widoku z `Index` method in Class metoda `HelloWorldController` klasy. Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony w klasie kontrolera. Zmiana `Index` metodę, aby zwrócić `View` obiektu, jak pokazano w następującym:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Ten kod szablonu widoku jest używane do generowania odpowiedzi HTML do przeglądarki. W projekcie, należy dodać szablon widoku, który można używać z `Index` metody. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz `Index` metody i kliknij przycisk **Dodaj widok**.

![](adding-a-view/_static/image1.png)

**Dodaj widok** pojawi się okno dialogowe. Pozostaw wartości domyślne, sposób ich i kliknij przycisk **Dodaj** przycisku:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* folder i *MvcMovie\Views\HelloWorld\Index.cshtml* plików są tworzone. Widać je w **Eksploratora rozwiązań**:

![](adding-a-view/_static/image3.png)

Poniższej przedstawiono *Index.cshtml* pliku, który został utworzony:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Dodaj kod HTML, w obszarze `<h2>` tagu. Zmodyfikowanego *MvcMovie\Views\HelloWorld\Index.cshtml* plików znajdują się poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Uruchom aplikację, a następnie przejdź do `HelloWorld` kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie dużo pracy; po prostu uruchomił instrukcję `return View()`, które określone metody należy używać pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie zostały jawnie określić nazwę pliku szablonu widoku, który będzie używany, ASP.NET MVC używa domyślnie *Index.cshtml* plik widoku w *\Views\HelloWorld* folderu. Na poniższym obrazie przedstawiono ciągu ustalonych w widoku.

![](adding-a-view/_static/image6.png)

Wygląda dość dobrze. Jednak Zauważ, że pasek tytułu w przeglądarce jest wyświetlany komunikat "Index" i big tytuł na stronie jest wyświetlany komunikat "Moja aplikacja MVC." Zmieńmy te z nich.

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i układ strony

Najpierw należy zmienić tytuł "Moja aplikacja MVC" w górnej części strony. Ten tekst jest wspólny dla każdej strony. Faktycznie jest stosowana w jednym miejscu w projekcie, nawet jeśli jest wyświetlana na każdej stronie w aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.cshtml* pliku. Ten plik jest nazywany *stronę układu* i jest udostępniony "powłoka" używanego w innych stron.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Szablony układu umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie. Uwaga `@RenderBody()` wiersz w pobliżu dolnej części pliku. `RenderBody` jest symbolem zastępczym, gdzie wszystkich stron tworzonych w specyficznych dla widoku wyświetlane, "zawinięty" na stronie układu. Zmień nagłówek tytuł szablonu układ z "Moja aplikacja MVC" do "MVC Movie App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Uruchom aplikację i zwróć uwagę, że teraz wyświetlany jest tekst "MVC Movie App". Kliknij przycisk **o** łącza i zobacz, jak ta strona pokazuje "MVC Movie App" zbyt. Byliśmy w stanie wprowadzić zmianę, jeden raz w szablonie układ i ma odzwierciedlać wszystkich stron w witrynie nowy tytuł.

![](adding-a-view/_static/image9.png)

Pełne  *\_Layout.cshtml* plików znajdują się poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teraz Zmieńmy tytuł strony indeksu (view).

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca, aby wprowadzić zmianę: po pierwsze, tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Wprowadzisz je nieco, dzięki czemu można zobaczyć, które fragmentem kodu zmienia której części aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Aby wskazać tytuł HTML, aby wyświetlić kod powyżej zestawy `Title` właściwość `ViewBag` obiektu (która znajduje się w *Index.cshtml* szablon widoku). Jeśli spojrzeć na kod źródłowy szablonu układ, można zauważyć, że ten szablon używa tej wartości w `<title>` elementu jako część `<head>` sekcji kodu HTML. W ten sposób można łatwo parametry są przekazywane innych między widoku szablonu i pliku układu.

Uruchom aplikację, a następnie przejdź do `http://localhost:xx/HelloWorld`. Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, prawdopodobnie przeglądasz zawartość z pamięci podręcznej. Naciśnij klawisze Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.)

Należy również zauważyć, jak zawartości *Index.cshtml* szablon widoku została scalona z  *\_Layout.cshtml* Wyświetl szablon i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Układ Szablony ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji.

![](adding-a-view/_static/image10.png)

Nasze trochę "dane" (w tym przypadku "Hello z naszych Wyświetl szablon!" komunikat) jest ustalony, mimo że. Aplikacja MVC ma "V" (view) i masz "C" (kontroler), ale nie "M" (model) jeszcze. Wkrótce, omówimy sposób tworzenia bazy danych i pobierania danych modelu z niego.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Zanim firma przejdź do bazy danych i porozmawiać na temat modeli jednak najpierw Omówmy przekazywania informacji z kontrolera do widoku. Klasy kontrolera są wywoływane w odpowiedzi na przychodzące żądanie adresu URL. Klasa kontrolera jest pisze się kod, który obsługuje przychodzące parametry, pobiera dane z bazy danych i ostatecznie decyduje o rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetl szablony następnie można za pomocą kontrolera do generowania i formatowanie odpowiedzi HTML do przeglądarki.

Kontrolery są odpowiedzialne za świadczenie dowolne dane i obiekty są wymagane dla szablonu widoku do renderowania odpowiedzi do przeglądarki. Szablon widoku nigdy nie należy wykonania logiki biznesowej lub bezpośredniej interakcji z bazą danych. Zamiast tego powinien działać tylko w przypadku danych, który znajduje się do niego przez kontroler. Obsługa tego "separacji" pomaga Twój kod będzie łatwiejszy w utrzymaniu i przejrzysty.

Obecnie `Welcome` metody akcji w `HelloWorldController` klasy przyjmuje `name` i `numTimes` parametru, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast kontrolera renderowania tej odpowiedzi jako ciąg znaków, zmienimy kontrolera, aby użyć szablonu widoku. Wyświetl szablon generuje odpowiedzi dynamicznej, oznacza to, czy musisz przekazać odpowiednich bitów danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić przez kontroler umieścić dane dynamiczne, wymagającym szablon widoku w `ViewBag` obiekt, który szablon widoku może uzyskać dostęp.

Wróć do *HelloWorldController.cs* pliku, a następnie zmień `Welcome` metody w celu dodania `Message` i `NumTimes` wartość `ViewBag` obiektu. `ViewBag` to obiekt dynamiczny, co oznacza, że możesz umieścić dowolne `ViewBag` obiekt nie ma zdefiniowanej właściwości do momentu coś znajdującym się w nim umieścić. Pełne *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Teraz `ViewBag` obiekt zawiera dane, które będą automatycznie przekazywane do widoku.

Następnie należy szablonu widoku Zapraszamy! W **debugowania** menu, wybierz opcję **kompilacji MvcMovie** się upewnić, że projekt jest skompilowany.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Kliknij prawym przyciskiem myszy wewnątrz `Welcome` metody i kliknij przycisk **Dodaj widok**. Oto, co **Dodaj widok** okno dialogowe wygląda następująco:

![](adding-a-view/_static/image13.png)

Kliknij przycisk **Dodaj**, a następnie dodaj następujący kod w `<h2>` elementu w nowym *Welcome.cshtml* pliku. Utworzysz pętlę, która jest wyświetlany komunikat "Hello" dowolną liczbę razy użytkownik wyświetla informację, że powinien on. Pełne *Welcome.cshtml* plików znajdują się poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Uruchom aplikację, a następnie przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i automatycznie przekazywane do kontrolera. Kontroler pakiety danych do `ViewBag` obiektu i przebiegi, które obiekt widoku. Widok następnie wyświetla określone dane jako kod HTML do użytkownika.

![](adding-a-view/_static/image14.png)

Cóż, to był rodzaju "M" dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy wyjaśniono konto i bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)
