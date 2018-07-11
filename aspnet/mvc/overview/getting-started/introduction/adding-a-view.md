---
title: Dodawanie widoku do aplikacji MVC
author: Rick-Anderson
description: Dodawanie widoku do aplikacji MVC
ms.author: riande
ms.date: 09/1721/2017
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 8b9ef79d630623019b22414ef730edffa5a83a09
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38166401"
---
<a name="adding-a-view"></a>Dodawanie widoku
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej sekcji zamierzasz zmodyfikować `HelloWorldController` klasy w celu użycia widoku pliki szablonu, aby prawidłowo zamykaj wystąpienia hermetyzacji proces generowania odpowiedzi HTML do klienta. 

Utworzysz widok szablonu pliku przy użyciu [aparatu widoku Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Szablony widoku razor mają *.cshtml* rozszerzenie pliku, a następnie podaj to elegancki sposób tworzenia kodu HTML w danych wyjściowych przy użyciu języka C#. Razor minimalizuje liczbę znaków i wymagane podczas pisania szablonu widoku naciśnięcia klawiszy i umożliwia szybkie, płynów, przepływ pracy kodowania.

Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony w klasie kontrolera. Zmiana `Index` metodę, aby zwrócić `View` obiektu, jak pokazano w poniższym kodzie:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index` Powyższej metody szablonu widoku jest używane do generowania odpowiedzi HTML do przeglądarki. Metody kontrolera (znany także jako [metod akcji](http://rachelappel.com/asp.net-mvc-actionresults-explained)), takie jak `Index` ogólnie zwracany przez metodę powyżej [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (lub klasą pochodną [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), nie pierwotnych typów, takich jak ciąg.

Kliknij prawym przyciskiem myszy *Views\HelloWorld* folder i kliknij przycisk **Dodaj**, następnie kliknij przycisk **strona widoku MVC 5 z układem (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
W **Określ nazwę dla elementu** okna dialogowego wprowadź *indeksu*, a następnie kliknij przycisk **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
W **wybierz stronę układu** okno dialogowe, zaakceptuj wartość domyślną  **\_Layout.cshtml** i kliknij przycisk **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
W oknie dialogowym powyżej *Views\Shared* wybraniu folderu w okienku po lewej stronie. Jeśli masz plik układu niestandardowego w innym folderze, można wybrać go. Omówimy plik układu w dalszej części tego samouczka

*MvcMovie\Views\HelloWorld\Index.cshtml* zostanie utworzony plik.

![](adding-a-view/_static/image4.png)

Dodaj następujący wyróżniony kod znaczników.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Kliknij prawym przyciskiem myszy *Index.cshtml* plik i wybierz **Pokaż w przeglądarce**.

![PI](adding-a-view/_static/image5.png)

Można również kliknięciu prawym przyciskiem myszy *Index.cshtml* plik i wybierz **widoku w narzędzia Page Inspector.** Zobacz [Samouczek narzędzie Page Inspector](../../views/using-page-inspector-in-aspnet-mvc.md) Aby uzyskać więcej informacji.

Alternatywnie Uruchom aplikację i przejdź do `HelloWorld` kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie dużo pracy; po prostu uruchomił instrukcję `return View()`, które określone metody należy używać pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie zostały jawnie określić nazwę pliku szablonu widoku, który będzie używany, ASP.NET MVC używa domyślnie *Index.cshtml* plik widoku w *\Views\HelloWorld* folderu. Na poniższym obrazie przedstawiono ciąg &quot;pozdrowienia z naszych Wyświetl szablon!&quot; zakodowane w widoku.

![](adding-a-view/_static/image6.png)

Wygląda dość dobrze. Jednak należy zauważyć, że pasek tytułu w przeglądarce pokazuje &quot;indeksu — Moje ASP.NET likacji "i big Data link w górnej części strony mówi"Nazwa aplikacji". W zależności od tego, jak małe wprowadzisz okna przeglądarki, konieczne może być kliknij trzy paski w prawym górnym rogu, aby zobaczyć do **Home**, **o**, **skontaktuj się z pomocą**, **Zarejestrować** i **Zaloguj** łącza.

## <a name="changing-views-and-layout-pages"></a>Zmienianie widoków i układ strony

Najpierw należy zmienić &quot;Nazwa aplikacji&quot; widocznego u góry strony. Ten tekst jest wspólny dla każdej strony. Faktycznie jest stosowana w jednym miejscu w projekcie, nawet jeśli jest wyświetlana na każdej stronie w aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.cshtml* pliku. Ten plik jest nazywany *stronę układu* i znajduje się w folderze udostępnionym, używanego przez innych stron.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Szablony układu umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie. Znajdź `@RenderBody()` wiersza. `RenderBody` jest symbolem zastępczym, gdzie wszystkie widoku specyficzne dla strony, możesz utworzyć pokaz, &quot;opakowane&quot; w stronę układu. Na przykład w przypadku wybrania **o** linku *Views\Home\About.cshtml* renderowania widoku w `RenderBody` metody.

Zmień zawartość elementu tytułu. Zmiana [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) w szablonie układ z &quot;Nazwa aplikacji&quot; do &quot;filmu MVC&quot; i kontroler z `Home` do `Movies`. Plik układu pełną znajdują się poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Uruchom aplikację i zwróć uwagę, że teraz mówi &quot;filmu MVC &quot;. Kliknij przycisk **o** link w którym zobaczysz, jak pokazuje tę stronę &quot;filmu MVC&quot;również. Byliśmy w stanie wprowadzić zmianę, jeden raz w szablonie układ i ma odzwierciedlać wszystkich stron w witrynie nowy tytuł.

![](adding-a-view/_static/image8.png)

Po pierwszym utworzeniu *Views\HelloWorld\Index.cshtml* pliku zawiera następujący kod:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Powyższy kod Razor jest jawne ustawienie strony układu. Sprawdź *widoków\\_ViewStart.cshtml* pliku zawiera dokładnie tych samych znaczników Razor. *[Widoków\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* plik definiuje układ typowe, która będzie używana we wszystkich widokach, dlatego możesz przekształcić w komentarz out lub Usuń kod *Views\HelloWorld\ Index.cshtml* pliku.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Możesz użyć `Layout` właściwości, aby ustawić inny układ widoku lub ustaw go na `null` więc plik układu nie będą używane.

Teraz Zmieńmy tytuł widoku indeksu.

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca, aby wprowadzić zmianę: po pierwsze, tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Wprowadzisz je nieco, dzięki czemu można zobaczyć, które fragmentem kodu zmienia której części aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Aby wskazać tytuł HTML, aby wyświetlić kod powyżej zestawy `Title` właściwość `ViewBag` obiektu (która znajduje się w *Index.cshtml* szablon widoku). Należy zauważyć, że szablon układu ( *Views\Shared\\_Layout.cshtml* ) używa tej wartości w `<title>` elementu jako część `<head>` sekcji kodu HTML, które wcześniej zmodyfikowany.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Za pomocą tego `ViewBag` podejście, możesz łatwo przekazać inne parametry między widoku szablonu i pliku układu.

Uruchom aplikację. Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, prawdopodobnie przeglądasz zawartość z pamięci podręcznej. Naciśnij klawisze Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewBag.Title` jest ustawiany *Index.cshtml* wyświetlić szablonu i dodatkowych &quot;— aplikacja filmu&quot; dodane w pliku układu.

Należy również zauważyć, jak zawartości *Index.cshtml* szablon widoku została scalona z  *\_Layout.cshtml* Wyświetl szablon i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Układ Szablony ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji.

![](adding-a-view/_static/image9.png)

Nasze trochę &quot;danych&quot; (w tym przypadku &quot;pozdrowienia z naszych Wyświetl szablon!&quot; wiadomości) jest ustalony, mimo że. Aplikacja MVC ma &quot;V&quot; (view) i masz &quot;C&quot; (kontroler), ale nie &quot;M&quot; (model) jeszcze. Wkrótce omówimy sposób utworzyć bazę danych i pobierania danych modelu z niego.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Zanim firma przejdź do bazy danych i porozmawiać na temat modeli jednak najpierw Omówmy przekazywania informacji z kontrolera do widoku. Klasy kontrolera są wywoływane w odpowiedzi na przychodzące żądanie adresu URL. Klasa kontrolera jest pisze się kod, który obsługuje przeglądarki przychodzących żądań, pobiera dane z bazy danych i ostatecznie decyduje o rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetl szablony następnie można za pomocą kontrolera do generowania i formatowanie odpowiedzi HTML do przeglądarki.

Kontrolery są odpowiedzialne za świadczenie dowolne dane i obiekty są wymagane dla szablonu widoku do renderowania odpowiedzi do przeglądarki. Najlepszym rozwiązaniem jest: **szablonu widoku nigdy nie może wykonać logikę biznesową ani bezpośredniej interakcji z bazą danych**. Zamiast tego szablonu widoku powinny działać tylko w przypadku danych, który znajduje się do niego przez kontroler. Utrzymywanie to &quot;separacji&quot; pomaga zapewnić Twój kod będzie łatwiejszy w utrzymaniu i przejrzysty, sprawdzalnego działa zgodnie.

Obecnie `Welcome` metody akcji w `HelloWorldController` klasy przyjmuje `name` i `numTimes` parametru, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast kontrolera renderowania tej odpowiedzi jako ciąg znaków, zmienimy kontrolera, aby użyć szablonu widoku. Wyświetl szablon generuje odpowiedzi dynamicznej, oznacza to, czy musisz przekazać odpowiednich bitów danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić przez kontroler umieścić dane dynamiczne (parametry) wymagającym szablon widoku w `ViewBag` obiekt, który szablon widoku może uzyskać dostęp.

Wróć do *HelloWorldController.cs* pliku, a następnie zmień `Welcome` metody w celu dodania `Message` i `NumTimes` wartość `ViewBag` obiektu. `ViewBag` to obiekt dynamiczny, co oznacza, że możesz umieścić dowolne `ViewBag` obiekt nie ma zdefiniowanej właściwości do momentu coś znajdującym się w nim umieścić. [System powiązań modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwanych parametrów (`name` i `numTimes`) z ciągu zapytania w pasku adresu do parametrów w metodzie. Pełne *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Teraz `ViewBag` obiekt zawiera dane, które będą automatycznie przekazywane do widoku. Następnie należy szablonu widoku Zapraszamy! W **kompilacji** menu, wybierz opcję **Kompiluj rozwiązanie** (lub Ctrl + Shift + B) aby upewnić się, że projekt jest skompilowany. Kliknij prawym przyciskiem myszy *Views\HelloWorld* folder i kliknij przycisk **Dodaj**, następnie kliknij przycisk **strona widoku MVC 5 z układem (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
W **Określ nazwę dla elementu** okna dialogowego wprowadź *powitalnej*, a następnie kliknij przycisk **OK**.   
  
W **wybierz stronę układu** okno dialogowe, zaakceptuj wartość domyślną  **\_Layout.cshtml** i kliknij przycisk **OK**.  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml* zostanie utworzony plik.

Zastąp kod znaczników w *Welcome.cshtml* pliku. Utworzysz pętlę, która wynika z &quot;Hello&quot; dowolną liczbę razy użytkownik odpowie, powinien on. Pełne *Welcome.cshtml* plików znajdują się poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Uruchom aplikację, a następnie przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i przekazywane do kontrolera, za pomocą [integratora modelu](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Kontroler pakiety danych do `ViewBag` obiektu i przebiegi, które obiekt widoku. Widok następnie wyświetla określone dane jako kod HTML do użytkownika.

![](adding-a-view/_static/image12.png)

W powyższym przykładzie użyliśmy `ViewBag` obiektu w celu przekazania danych z kontrolera do widoku. W dalszej części samouczka użyjemy modelu widoku do przekazywania danych za pomocą kontrolera do widoku. Widok modelu sposobem przekazywania danych to zwykle znacznie preferowane podejście pakiet widoku. Zobacz wpis na blogu [V silnie Typizowane widoki dynamiczne](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) Aby uzyskać więcej informacji. 

Dobrze, który był rodzajem elementu &quot;M&quot; dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy wyjaśniono konto i bazę danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-a-controller.md)
> [dalej](adding-a-model.md)
