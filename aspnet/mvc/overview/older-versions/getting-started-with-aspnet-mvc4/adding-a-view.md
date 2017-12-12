---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Dodawanie widoku | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Uwaga: Zaktualizowaną wersję tego samouczka jest dostępnych tutaj używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, znacznie prostsza do wykonania i demonstracją..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4309290294b28d4c177e0193719bcff4b3f2a8cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view"></a>Dodawanie widoku
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.


W tej sekcji zamierzasz zmodyfikować `HelloWorldController` klasę, aby użyć widoku pliki szablonu, aby bezpośrednio Hermetyzowanie proces generowania odpowiedzi HTML do klienta.

Utworzysz widok szablonu pliku za pomocą [aparatu widoku Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) wprowadzonego w programie ASP.NET MVC 3. Szablony razor na podstawie widoku mają *.cshtml* rozszerzenie pliku i zapewnić atrakcyjny sposób utworzenia kodu HTML w danych wyjściowych przy użyciu języka C#. Razor minimalizuje liczbę znaków i wymagane podczas zapisywania szablonu widoku naciśnięć klawiszy i umożliwia szybkie, płynu kodowania przepływu pracy.

Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony klasy kontrolera. Zmień `Index` metodę, aby zwrócić `View` obiektów, jak pokazano w poniższym kodzie:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index` Metod powyżej szablonu widok używane do generowania odpowiedzi HTML do przeglądarki. Metody kontrolera (znanej także jako [metod akcji](http://rachelappel.com/asp.net-mvc-actionresults-explained)), takich jak `Index` zazwyczaj zwracany przez metodę powyżej [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (lub klasą pochodną [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)), typy pierwotne nie, takich jak ciąg.

W projekcie Dodaj szablon widoku, który można używać z `Index` metody. Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz `Index` — metoda i kliknij przycisk **Dodaj widok**.

![](adding-a-view/_static/image1.png)

**Dodaj widok** zostanie wyświetlone okno dialogowe. Pozostaw wartości domyślne są a kliknij przycisk **Dodaj** przycisk:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* folderu i *MvcMovie\Views\HelloWorld\Index.cshtml* plików są tworzone. Można wyświetlić je w **Eksploratora rozwiązań**:

![](adding-a-view/_static/image3.png)

Poniżej przedstawiono *Index.cshtml* pliku, który został utworzony:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Dodaj poniższy kod HTML pod `<h2>` tagu.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Pełną *MvcMovie\Views\HelloWorld\Index.cshtml* plików są wyświetlane poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Jeśli używasz programu Visual Studio 2012, w Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy kliknij *Index.cshtml* plik i wybierz **widoku w narzędzie Page Inspector**.

![PI](adding-a-view/_static/image5.png)

[Samouczek narzędzie Page Inspector](../../views/using-page-inspector-in-aspnet-mvc.md) ma więcej informacji na temat tego nowego narzędzia.

Możesz też uruchomić aplikację i przejdź do `HelloWorld` kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie wykonać dużo pracy; po prostu uruchomił instrukcję `return View()`, który określony metody należy użyć pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie został jawnie określić nazwę pliku szablonu widok do użycia, ASP.NET MVC używa domyślnie *Index.cshtml* pliku widoku w *\Views\HelloWorld* folderu. Na poniższym obrazie pokazano ciąg &quot;Hello z naszych szablonu widoku!&quot; ustalony w widoku.

![](adding-a-view/_static/image6.png)

Wygląda bardzo dobre. Jednak należy zauważyć, że wskazuje paska tytułu w przeglądarce &quot;indeksu Moje A ASP.NET&quot; i duży łącze w górnej części strony widać polecenie &quot;tutaj znak logo.&quot; Poniżej &quot;tutaj logo.&quot; łącza są o rejestracji i dziennika w łącza, pod który stanowi łącze do strony głównej i skontaktuj się z stron. Zmieńmy niektóre z nich.

## <a name="changing-views-and-layout-pages"></a>Zmiana widoków i układ strony

Najpierw należy zmienić &quot;tutaj logo.&quot; tytuł w górnej części strony. Ten tekst jest wspólny dla każdej strony. Faktycznie jest stosowana w tylko jednym miejscu w projekcie, nawet jeśli wygląda na to, na wszystkich stronach w aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.cshtml* pliku. Ten plik jest nazywany *układ strony* i jest udostępniony &quot;powłoki&quot; korzystające z innych stron.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Szablony układu umożliwiają określ układ kontenera HTML witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie sieci. Znajdź `@RenderBody()` wiersza. `RenderBody`jest symbol zastępczy, gdzie wszystkie widoku specyficzne dla stron, możesz utworzyć pokaz, &quot;opakowana&quot; na stronie układu. Na przykład w przypadku wybrania łącza o *Views\Home\About.cshtml* renderowania widoku wewnątrz `RenderBody` metody.

Zmiana pozycji tytułu witryny w szablon układu z &quot;tutaj znak logo&quot; do &quot;MVC Movie&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Zastąp zawartość elementu tytuł następujący kod:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Uruchom aplikację i zwróć uwagę, że teraz mówi &quot;MVC Movie &quot;. Kliknij przycisk **o** łącza i zostanie wyświetlony, jak ta strona przedstawia &quot;MVC Movie&quot;, zbyt. Udało się zmienić raz w szablonie układ i mieć wszystkich stron w witrynie uwzględniać nowy tytuł.

![](adding-a-view/_static/image8.png)

Teraz Zmieńmy tytuł widoku indeksu.

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca do dokonania zmiany: najpierw tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Należy podjąć ich nieco inne pozwala zobaczyć, które fragmentem kodu zmienia której części aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Aby wskazać tytuł HTML, aby wyświetlić kod nad zestawy `Title` właściwość `ViewBag` obiektu (w *Index.cshtml* wyświetlanie szablonu). Jeśli przyjrzymy się ponownie kod źródłowy szablon układu, można zauważyć, że szablon używa tej wartości w `<title>` element jako część `<head>` sekcji kodu HTML, które firma Microsoft wcześniej zmodyfikowane. Za pomocą tej `ViewBag` podejście, można łatwo przekazać innych parametrów między widok szablonu i pliku układu.

Uruchom aplikację i przejdź do `http://localhost:xx/HelloWorld`. Należy zauważyć, że tytuł przeglądarki, nagłówek głównej i dodatkowej nagłówki zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, użytkownik może mieć możliwość wyświetlania zawartości w pamięci podręcznej. Naciśnij klawisz Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewBag.Title` ustawiliśmy *Index.cshtml* wyświetlić szablonu i dodatkowych &quot;-Movie App&quot; dodane w pliku układu.

Należy również zauważyć, jak zawartości w *Index.cshtml* Wyświetl szablon został scalony z  *\_Layout.cshtml* szablon widoku i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Szablony układu ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji.

![](adding-a-view/_static/image9.png)

Nasze niewielki &quot;danych&quot; (w tym przypadku &quot;Hello z naszych szablonu widoku!&quot; wiadomości) jest ustalony, mimo że. Aplikacja MVC ma &quot;V&quot; (Widok) i masz &quot;C&quot; (kontroler), lecz nie &quot;M&quot; (model) jeszcze. Wkrótce, zostanie omówiony sposób utworzyć bazę danych i pobrać modelu danych.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Przed możemy przejdź do bazy danych i porozmawiać na temat modeli, umożliwia najpierw porozmawiać na temat przekazywania informacji z kontrolera do widoku. Klasy kontrolera są wywoływane w odpowiedzi na żądania przychodzącego adresu URL. Klasa kontrolera jest, gdzie napisać kod obsługujący przeglądarki przychodzących żądań, pobiera dane z bazy danych i ostatecznie decyduje o tym, jakiego rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetl szablony mogą następnie używane z kontrolera do generowania i format odpowiedzi HTML do przeglądarki.

Kontrolery spoczywa odpowiedzialność za zapewnienie, niezależnie od danych lub obiekty są wymagane dla szablonu widoku do renderowania odpowiedzi do przeglądarki. Najlepszym rozwiązaniem: **szablonu widoku nigdy nie może wykonać logiki biznesowej lub bezpośredniej interakcji z bazą danych**. Zamiast tego szablonu widoku powinny działać tylko z danymi, które jest podany przez kontroler. Utrzymywanie to &quot;separacji&quot; pomaga zapewnić kodu czyste, testować i łatwiejsze do utrzymania.

Obecnie `Welcome` metody akcji w `HelloWorldController` klasy przyjmuje `name` i `numTimes` parametr, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast renderowania tej odpowiedzi jako ciąg kontrolera, umożliwia zmianę kontrolera zamiast tego użyć szablonu widoku. Szablon widoku wygeneruje odpowiedzi dynamicznych, co oznacza, że trzeba przekazać odpowiednich bitów danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić przez kontroler put danych dynamicznych (parametry), które wymaga Wyświetl szablon o `ViewBag` obiekt, który szablon widoku można następnie uzyskać dostęp.

Wróć do *HelloWorldController.cs* plików i zmień `Welcome` metody w celu dodania `Message` i `NumTimes` do wartości `ViewBag` obiektu. `ViewBag`jest to obiekt dynamiczny, co oznacza, że możesz umieścić dowolne `ViewBag` obiekt nie ma zdefiniowanej właściwości dopóki coś wewnątrz put. [System powiązanie modelu platformy ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwane parametry (`name` i `numTimes`) z ciągu zapytania w pasku adresu w parametrach w metodę. Pełną *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Teraz `ViewBag` obiekt zawiera dane, które będą automatycznie przekazywane do widoku.

Następnie należy szablonu widoku Zapraszamy! W **kompilacji** menu, wybierz opcję **kompilacji MvcMovie** się upewnić, że projekt jest kompilowany.

Kliknij prawym przyciskiem myszy wewnątrz `Welcome` — metoda i kliknij przycisk **Dodaj widok**.

![](adding-a-view/_static/image10.png)

Oto, co **Dodaj widok** okno dialogowe wygląda następująco:

![](adding-a-view/_static/image11.png)

Kliknij przycisk **Dodaj**, a następnie dodaj następujący kod w obszarze `<h2>` elementu w nowym *Welcome.cshtml* pliku. Utworzysz pętli, informujący o &quot;Hello&quot; tyle razy, ile użytkownik odpowie powinno. Pełną *Welcome.cshtml* plików są wyświetlane poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Uruchom aplikację, a następnie przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i przekazywane do kontrolera przy użyciu [integratora modelu](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Kontroler pakiety danych w `ViewBag` obiekt i przekazuje, które obiekt do widoku. Widok następnie wyświetla dane jako kodu HTML dla użytkownika.

![](adding-a-view/_static/image12.png)

W powyższym przykładzie użyliśmy `ViewBag` obiektu do przekazywania danych z kontrolera do widoku. Później w samouczku, użyjemy modelu widoku do przekazywania danych z kontrolera do widoku. Podejście modelu widoku do przekazywania danych jest zwykle znacznie preferowana za pośrednictwem podejście pakiet widoku. Zobacz wpis w blogu [V silnie Typizowanej widoki dynamiczne](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) Aby uzyskać więcej informacji.

Źródło, które były rodzaju z &quot;M&quot; dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy samouczka jest i utworzyć bazę danych filmów.

>[!div class="step-by-step"]
[Poprzednie](adding-a-controller.md)
[dalej](adding-a-model.md)
