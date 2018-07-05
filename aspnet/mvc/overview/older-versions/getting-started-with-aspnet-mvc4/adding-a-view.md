---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Dodawanie widoku | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Zaktualizowaną wersję w tym samouczku jest dostępna w tym miejscu używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0974cd2e06ed86c736214944a29a5a1eab8af50b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391512"
---
<a name="adding-a-view"></a>Dodawanie widoku
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji zamierzasz zmodyfikować `HelloWorldController` klasy w celu użycia widoku pliki szablonu, aby prawidłowo zamykaj wystąpienia hermetyzacji proces generowania odpowiedzi HTML do klienta.

Utworzysz widok szablonu pliku przy użyciu [aparatu widoku Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) wprowadzone w programie ASP.NET MVC 3. Szablony widoku razor mają *.cshtml* rozszerzenie pliku, a następnie podaj to elegancki sposób tworzenia kodu HTML w danych wyjściowych przy użyciu języka C#. Razor minimalizuje liczbę znaków i wymagane podczas pisania szablonu widoku naciśnięcia klawiszy i umożliwia szybkie, płynów, przepływ pracy kodowania.

Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony w klasie kontrolera. Zmiana `Index` metodę, aby zwrócić `View` obiektu, jak pokazano w poniższym kodzie:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index` Powyższej metody szablonu widoku jest używane do generowania odpowiedzi HTML do przeglądarki. Metody kontrolera (znany także jako [metod akcji](http://rachelappel.com/asp.net-mvc-actionresults-explained)), takie jak `Index` ogólnie zwracany przez metodę powyżej [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (lub klasą pochodną [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), nie pierwotnych typów, takich jak ciąg.

W projekcie, należy dodać szablon widoku, który można używać z `Index` metody. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz `Index` metody i kliknij przycisk **Dodaj widok**.

![](adding-a-view/_static/image1.png)

**Dodaj widok** pojawi się okno dialogowe. Pozostaw wartości domyślne, sposób ich i kliknij przycisk **Dodaj** przycisku:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* folder i *MvcMovie\Views\HelloWorld\Index.cshtml* plików są tworzone. Widać je w **Eksploratora rozwiązań**:

![](adding-a-view/_static/image3.png)

Poniższej przedstawiono *Index.cshtml* pliku, który został utworzony:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Dodaj poniższy kod HTML pod `<h2>` tagu.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Pełne *MvcMovie\Views\HelloWorld\Index.cshtml* plików znajdują się poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Korzystania z programu Visual Studio 2012, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *Index.cshtml* plik i wybierz **widoku w narzędzia Page Inspector**.

![PI](adding-a-view/_static/image5.png)

[Samouczek narzędzie Page Inspector](../../views/using-page-inspector-in-aspnet-mvc.md) zawiera więcej informacji na temat tego nowego narzędzia.

Alternatywnie Uruchom aplikację i przejdź do `HelloWorld` kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie dużo pracy; po prostu uruchomił instrukcję `return View()`, które określone metody należy używać pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie zostały jawnie określić nazwę pliku szablonu widoku, który będzie używany, ASP.NET MVC używa domyślnie *Index.cshtml* plik widoku w *\Views\HelloWorld* folderu. Na poniższym obrazie przedstawiono ciąg &quot;pozdrowienia z naszych Wyświetl szablon!&quot; zakodowane w widoku.

![](adding-a-view/_static/image6.png)

Wygląda dość dobrze. Jednak należy zauważyć, że pasek tytułu w przeglądarce pokazuje &quot;indeksu Moje ASP.NET A&quot; i mówi big łącze w górnej części strony &quot;Twoje logo.&quot; Poniżej &quot;tutaj logo.&quot; łącze o są rejestracji i dziennika w linkach, a poniżej prowadzący do strony głównej i skontaktuj się z stron. Zmieńmy niektóre z nich.

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i układ strony

Najpierw należy zmienić &quot;tutaj logo.&quot; tytułu w górnej części strony. Ten tekst jest wspólny dla każdej strony. Faktycznie jest stosowana w jednym miejscu w projekcie, nawet jeśli jest wyświetlana na każdej stronie w aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.cshtml* pliku. Ten plik jest nazywany *stronę układu* i wspólnie &quot;powłoki&quot; używanego przez innych stron.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Szablony układu umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie. Znajdź `@RenderBody()` wiersza. `RenderBody` jest symbolem zastępczym, gdzie wszystkie widoku specyficzne dla strony, możesz utworzyć pokaz, &quot;opakowane&quot; w stronę układu. Na przykład, jeśli wybierzesz łącze informacje *Views\Home\About.cshtml* renderowania widoku wewnątrz `RenderBody` metody.

Zmień nagłówek tytuł witryny w szablonie układ z &quot;Twoje logo&quot; do &quot;filmu MVC&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Zastąp zawartość elementu tytuł następującym kodem:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Uruchom aplikację i zwróć uwagę, że teraz mówi &quot;filmu MVC &quot;. Kliknij przycisk **o** link w którym zobaczysz, jak pokazuje tę stronę &quot;filmu MVC&quot;również. Byliśmy w stanie wprowadzić zmianę, jeden raz w szablonie układ i ma odzwierciedlać wszystkich stron w witrynie nowy tytuł.

![](adding-a-view/_static/image8.png)

Teraz Zmieńmy tytuł widoku indeksu.

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca, aby wprowadzić zmianę: po pierwsze, tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Wprowadzisz je nieco, dzięki czemu można zobaczyć, które fragmentem kodu zmienia której części aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Aby wskazać tytuł HTML, aby wyświetlić kod powyżej zestawy `Title` właściwość `ViewBag` obiektu (która znajduje się w *Index.cshtml* szablon widoku). Jeśli spojrzeć na kod źródłowy szablonu układ, można zauważyć, że ten szablon używa tej wartości w `<title>` elementu jako część `<head>` sekcji kodu HTML, które wcześniej zmodyfikowany. Za pomocą tego `ViewBag` podejście, możesz łatwo przekazać inne parametry między widoku szablonu i pliku układu.

Uruchom aplikację, a następnie przejdź do `http://localhost:xx/HelloWorld`. Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, prawdopodobnie przeglądasz zawartość z pamięci podręcznej. Naciśnij klawisze Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewBag.Title` jest ustawiany *Index.cshtml* wyświetlić szablonu i dodatkowych &quot;— aplikacja filmu&quot; dodane w pliku układu.

Należy również zauważyć, jak zawartości *Index.cshtml* szablon widoku została scalona z  *\_Layout.cshtml* Wyświetl szablon i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Układ Szablony ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji.

![](adding-a-view/_static/image9.png)

Nasze trochę &quot;danych&quot; (w tym przypadku &quot;pozdrowienia z naszych Wyświetl szablon!&quot; wiadomości) jest ustalony, mimo że. Aplikacja MVC ma &quot;V&quot; (view) i masz &quot;C&quot; (kontroler), ale nie &quot;M&quot; (model) jeszcze. Wkrótce, omówimy sposób tworzenia bazy danych i pobierania danych modelu z niego.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Zanim firma przejdź do bazy danych i porozmawiać na temat modeli jednak najpierw Omówmy przekazywania informacji z kontrolera do widoku. Klasy kontrolera są wywoływane w odpowiedzi na przychodzące żądanie adresu URL. Klasa kontrolera jest pisze się kod, który obsługuje przeglądarki przychodzących żądań, pobiera dane z bazy danych i ostatecznie decyduje o rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetl szablony następnie można za pomocą kontrolera do generowania i formatowanie odpowiedzi HTML do przeglądarki.

Kontrolery są odpowiedzialne za świadczenie dowolne dane i obiekty są wymagane dla szablonu widoku do renderowania odpowiedzi do przeglądarki. Najlepszym rozwiązaniem jest: **szablonu widoku nigdy nie może wykonać logikę biznesową ani bezpośredniej interakcji z bazą danych**. Zamiast tego szablonu widoku powinny działać tylko w przypadku danych, który znajduje się do niego przez kontroler. Utrzymywanie to &quot;separacji&quot; pomaga zapewnić Twój kod będzie łatwiejszy w utrzymaniu i przejrzysty, sprawdzalnego działa zgodnie.

Obecnie `Welcome` metody akcji w `HelloWorldController` klasy przyjmuje `name` i `numTimes` parametru, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast kontrolera renderowania tej odpowiedzi jako ciąg znaków, zmienimy kontrolera, aby użyć szablonu widoku. Wyświetl szablon generuje odpowiedzi dynamicznej, oznacza to, czy musisz przekazać odpowiednich bitów danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić przez kontroler umieścić dane dynamiczne (parametry) wymagającym szablon widoku w `ViewBag` obiekt, który szablon widoku może uzyskać dostęp.

Wróć do *HelloWorldController.cs* pliku, a następnie zmień `Welcome` metody w celu dodania `Message` i `NumTimes` wartość `ViewBag` obiektu. `ViewBag` to obiekt dynamiczny, co oznacza, że możesz umieścić dowolne `ViewBag` obiekt nie ma zdefiniowanej właściwości do momentu coś znajdującym się w nim umieścić. [System powiązań modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwanych parametrów (`name` i `numTimes`) z ciągu zapytania w pasku adresu do parametrów w metodzie. Pełne *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Teraz `ViewBag` obiekt zawiera dane, które będą automatycznie przekazywane do widoku.

Następnie należy szablonu widoku Zapraszamy! W **kompilacji** menu, wybierz opcję **kompilacji MvcMovie** się upewnić, że projekt jest skompilowany.

Kliknij prawym przyciskiem myszy wewnątrz `Welcome` metody i kliknij przycisk **Dodaj widok**.

![](adding-a-view/_static/image10.png)

Oto, co **Dodaj widok** okno dialogowe wygląda następująco:

![](adding-a-view/_static/image11.png)

Kliknij przycisk **Dodaj**, a następnie dodaj następujący kod w `<h2>` elementu w nowym *Welcome.cshtml* pliku. Utworzysz pętlę, która wynika z &quot;Hello&quot; dowolną liczbę razy użytkownik odpowie, powinien on. Pełne *Welcome.cshtml* plików znajdują się poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Uruchom aplikację, a następnie przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i przekazywane do kontrolera, za pomocą [integratora modelu](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Kontroler pakiety danych do `ViewBag` obiektu i przebiegi, które obiekt widoku. Widok następnie wyświetla określone dane jako kod HTML do użytkownika.

![](adding-a-view/_static/image12.png)

W powyższym przykładzie użyliśmy `ViewBag` obiektu w celu przekazania danych z kontrolera do widoku. Ostatni w tym samouczku, użyjemy modelu widoku do przekazywania danych za pomocą kontrolera do widoku. Widok modelu sposobem przekazywania danych to zwykle znacznie preferowane podejście pakiet widoku. Zobacz wpis na blogu [V silnie Typizowane widoki dynamiczne](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) Aby uzyskać więcej informacji.

Dobrze, który był rodzajem elementu &quot;M&quot; dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy wyjaśniono konto i bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)
