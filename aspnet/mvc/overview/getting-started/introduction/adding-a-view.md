---
title: Dodawanie do aplikacji MVC widok
author: Rick-Anderson
description: Dodawanie do aplikacji MVC widok
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: d273eb5e99da6c6b7678e03b1a8973041113744c
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/09/2018
---
<a name="adding-a-view"></a>Dodawanie widoku
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

W tej sekcji zamierzasz zmodyfikować `HelloWorldController` klasę, aby użyć widoku pliki szablonu, aby bezpośrednio Hermetyzowanie proces generowania odpowiedzi HTML do klienta. 

Utworzysz widok szablonu pliku za pomocą [aparatu widoku Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Szablony razor na podstawie widoku mają *.cshtml* rozszerzenie pliku i zapewnić atrakcyjny sposób utworzenia kodu HTML w danych wyjściowych przy użyciu języka C#. Razor minimalizuje liczbę znaków i wymagane podczas zapisywania szablonu widoku naciśnięć klawiszy i umożliwia szybkie, płynu kodowania przepływu pracy.

Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony klasy kontrolera. Zmień `Index` metodę, aby zwrócić `View` obiektów, jak pokazano w poniższym kodzie:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index` Metod powyżej szablonu widok używane do generowania odpowiedzi HTML do przeglądarki. Metody kontrolera (znanej także jako [metod akcji](http://rachelappel.com/asp.net-mvc-actionresults-explained)), takich jak `Index` zazwyczaj zwracany przez metodę powyżej [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (lub klasą pochodną [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)), typy pierwotne nie, takich jak ciąg.

Kliknij prawym przyciskiem myszy *Views\HelloWorld* folder i kliknij przycisk **Dodaj**, następnie kliknij przycisk **strona widoku MVC 5 z układem (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
W **Określ nazwę dla elementu** okna dialogowego wprowadź *indeksu*, a następnie kliknij przycisk **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
W **wybierz stronę układu** okna dialogowego, zaakceptuj wartość domyślną  **\_Layout.cshtml** i kliknij przycisk **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
W oknie dialogowym powyżej *Views\Shared* wybraniu folderu w okienku po lewej stronie. Jeśli masz układu niestandardowego pliku w innym folderze, można wybrać go. Będzie omawianiu pliku układu później w samouczku

*MvcMovie\Views\HelloWorld\Index.cshtml* tworzony jest plik.

![](adding-a-view/_static/image4.png)

Dodaj następujący wyróżniony kod znaczników.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Kliknij prawym przyciskiem myszy *Index.cshtml* plik i wybierz **Wyświetl w przeglądarce**.

![PI](adding-a-view/_static/image5.png)

Użytkownik może również kliknij prawym przyciskiem myszy *Index.cshtml* plik i wybierz **widoku w narzędzie Page Inspector.** Zobacz [Samouczek narzędzie Page Inspector](../../views/using-page-inspector-in-aspnet-mvc.md) Aby uzyskać więcej informacji.

Możesz też uruchomić aplikację i przejdź do `HelloWorld` kontrolera (`http://localhost:xxxx/HelloWorld`). `Index` Metody w kontrolerze nie wykonać dużo pracy; po prostu uruchomił instrukcję `return View()`, który określony metody należy użyć pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie został jawnie określić nazwę pliku szablonu widok do użycia, ASP.NET MVC używa domyślnie *Index.cshtml* pliku widoku w *\Views\HelloWorld* folderu. Na poniższym obrazie pokazano ciąg &quot;Hello z naszych szablonu widoku!&quot; ustalony w widoku.

![](adding-a-view/_static/image6.png)

Wygląda bardzo dobre. Jednak zauważyć, że przeglądarki pasek tytułu zawiera &quot;indeksu - Moje nazwę ASP.NET "i duży łącze w górnej części strony widać polecenie"Nazwa aplikacji". W zależności od tego, jak mała wprowadzeniu okna przeglądarki, może zajść potrzeba kliknięcia przycisku trzy paski w prawym górnym rogu, aby wyświetlić do **Home**, **o**, **skontaktuj się z**, **Zarejestrować** i **Zaloguj** łącza.

## <a name="changing-views-and-layout-pages"></a>Zmiana widoków i układ strony

Najpierw należy zmienić &quot;Nazwa aplikacji&quot; łącze w górnej części strony. Ten tekst jest wspólny dla każdej strony. Faktycznie jest stosowana w tylko jednym miejscu w projekcie, nawet jeśli wygląda na to, na wszystkich stronach w aplikacji. Przejdź do */widoków/Shared* folderu w **Eksploratora rozwiązań** , a następnie otwórz  *\_Layout.cshtml* pliku. Ten plik jest nazywany *układ strony* i folderu udostępnionego, który użyć innych stron.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Szablony układu umożliwiają określ układ kontenera HTML witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie sieci. Znajdź `@RenderBody()` wiersza. `RenderBody`jest symbol zastępczy, gdzie wszystkie widoku specyficzne dla stron, możesz utworzyć pokaz, &quot;opakowana&quot; na stronie układu. Na przykład w przypadku wybrania **o** łącza, *Views\Home\About.cshtml* renderowania widoku wewnątrz `RenderBody` metody.

Zmienić zawartość elementu tytułu. Zmień [ActionLink](https://msdn.microsoft.com/en-us/library/dd504972(v=vs.108).aspx) w szablonie układu z &quot;Nazwa aplikacji&quot; do &quot;MVC Movie&quot; i kontroler z `Home` do `Movies`. Plik układu pełną przedstawiono poniżej:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Uruchom aplikację i zwróć uwagę, że teraz mówi &quot;MVC Movie &quot;. Kliknij przycisk **o** łącza i zostanie wyświetlony, jak ta strona przedstawia &quot;MVC Movie&quot;, zbyt. Udało się zmienić raz w szablonie układ i mieć wszystkich stron w witrynie uwzględniać nowy tytuł.

![](adding-a-view/_static/image8.png)

Po pierwsze utworzyliśmy *Views\HelloWorld\Index.cshtml* pliku zawiera następujący kod:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Powyższy kod Razor jawnie ustawia strony układu. Sprawdź *widoków\\_ViewStart.cshtml* pliku zawiera dokładnie tego samego znaczników Razor.  *[Widoków\\_ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)*  pliku definiuje typowe układu, która będzie używana we wszystkich widokach, dlatego można komentarz out lub Usuń ten kod z *Views\HelloWorld\ Index.cshtml* pliku.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Można użyć `Layout` właściwość, aby ustawić inny układ widok lub ustaw ją na `null` tak ma układ pliku będzie używany.

Teraz Zmieńmy tytuł widoku indeksu.

Otwórz *MvcMovie\Views\HelloWorld\Index.cshtml*. Istnieją dwa miejsca do dokonania zmiany: najpierw tekst wyświetlany w tytule przeglądarki, a następnie w nagłówku dodatkowej ( `<h2>` elementu). Należy podjąć ich nieco inne pozwala zobaczyć, które fragmentem kodu zmienia której części aplikacji.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Aby wskazać tytuł HTML, aby wyświetlić kod nad zestawy `Title` właściwość `ViewBag` obiektu (w *Index.cshtml* wyświetlanie szablonu). Zwróć uwagę, że szablon układu ( *Views\Shared\\_Layout.cshtml* ) używa tej wartości w `<title>` element jako część `<head>` sekcji kodu HTML, które firma Microsoft wcześniej zmodyfikowane.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Za pomocą tej `ViewBag` podejście, można łatwo przekazać innych parametrów między widok szablonu i pliku układu.

Uruchom aplikację. Należy zauważyć, że tytuł przeglądarki, nagłówek głównej i dodatkowej nagłówki zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, użytkownik może mieć możliwość wyświetlania zawartości w pamięci podręcznej. Naciśnij klawisz Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewBag.Title` ustawiliśmy *Index.cshtml* wyświetlić szablonu i dodatkowych &quot;-Movie App&quot; dodane w pliku układu.

Należy również zauważyć, jak zawartości w *Index.cshtml* Wyświetl szablon został scalony z  *\_Layout.cshtml* szablon widoku i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Szablony układu ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji.

![](adding-a-view/_static/image9.png)

Nasze niewielki &quot;danych&quot; (w tym przypadku &quot;Hello z naszych szablonu widoku!&quot; wiadomości) jest ustalony, mimo że. Aplikacja MVC ma &quot;V&quot; (Widok) i masz &quot;C&quot; (kontroler), lecz nie &quot;M&quot; (model) jeszcze. Wkrótce, zostanie omówiony sposób tworzenia bazy danych i pobrać modelu danych.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Przed możemy przejdź do bazy danych i porozmawiać na temat modeli, umożliwia najpierw porozmawiać na temat przekazywania informacji z kontrolera do widoku. Klasy kontrolera są wywoływane w odpowiedzi na żądania przychodzącego adresu URL. Klasa kontrolera jest, gdzie napisać kod obsługujący przeglądarki przychodzących żądań, pobiera dane z bazy danych i ostatecznie decyduje o tym, jakiego rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetl szablony mogą następnie używane z kontrolera do generowania i format odpowiedzi HTML do przeglądarki.

Kontrolery spoczywa odpowiedzialność za zapewnienie, niezależnie od danych lub obiekty są wymagane dla szablonu widoku do renderowania odpowiedzi do przeglądarki. Najlepszym rozwiązaniem: **szablonu widoku nigdy nie może wykonać logiki biznesowej lub bezpośredniej interakcji z bazą danych**. Zamiast tego szablonu widoku powinny działać tylko z danymi, które jest podany przez kontroler. Utrzymywanie to &quot;separacji&quot; pomaga zapewnić kodu czyste, testować i łatwiejsze do utrzymania.

Obecnie `Welcome` metody akcji w `HelloWorldController` klasy przyjmuje `name` i `numTimes` parametr, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast renderowania tej odpowiedzi jako ciąg kontrolera, umożliwia zmianę kontrolera zamiast tego użyć szablonu widoku. Szablon widoku wygeneruje odpowiedzi dynamicznych, co oznacza, że trzeba przekazać odpowiednich bitów danych z kontrolera do widoku w celu wygenerowania odpowiedzi. Można to zrobić przez kontroler put danych dynamicznych (parametry), które wymaga Wyświetl szablon o `ViewBag` obiekt, który szablon widoku można następnie uzyskać dostęp.

Wróć do *HelloWorldController.cs* plików i zmień `Welcome` metody w celu dodania `Message` i `NumTimes` do wartości `ViewBag` obiektu. `ViewBag`jest to obiekt dynamiczny, co oznacza, że możesz umieścić dowolne `ViewBag` obiekt nie ma zdefiniowanej właściwości dopóki coś wewnątrz put. [System powiązanie modelu platformy ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatycznie mapuje nazwane parametry (`name` i `numTimes`) z ciągu zapytania w pasku adresu w parametrach w metodę. Pełną *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Teraz `ViewBag` obiekt zawiera dane, które będą automatycznie przekazywane do widoku. Następnie należy szablonu widoku Zapraszamy! W **kompilacji** menu, wybierz opcję **Kompiluj rozwiązanie** (lub Ctrl + Shift + B) do upewnij się, że projekt jest kompilowany. Kliknij prawym przyciskiem myszy *Views\HelloWorld* folder i kliknij przycisk **Dodaj**, następnie kliknij przycisk **strona widoku MVC 5 z układem (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
W **Określ nazwę dla elementu** okna dialogowego wprowadź *powitalnej*, a następnie kliknij przycisk **OK**.   
  
W **wybierz stronę układu** okna dialogowego, zaakceptuj wartość domyślną  **\_Layout.cshtml** i kliknij przycisk **OK**.  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml* tworzony jest plik.

Zastąp kod znaczników w *Welcome.cshtml* pliku. Utworzysz pętli, informujący o &quot;Hello&quot; tyle razy, ile użytkownik odpowie powinno. Pełną *Welcome.cshtml* plików są wyświetlane poniżej.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Uruchom aplikację, a następnie przejdź do następującego adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Teraz dane są pobierane z adresu URL i przekazywane do kontrolera przy użyciu [integratora modelu](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Kontroler pakiety danych w `ViewBag` obiekt i przekazuje, które obiekt do widoku. Widok następnie wyświetla dane jako kodu HTML dla użytkownika.

![](adding-a-view/_static/image12.png)

W powyższym przykładzie użyliśmy `ViewBag` obiektu do przekazywania danych z kontrolera do widoku. Później w samouczku używamy model widoku do przekazywania danych z kontrolera do widoku. Podejście modelu widoku do przekazywania danych jest zwykle znacznie preferowana za pośrednictwem podejście pakiet widoku. Zobacz wpis w blogu [V silnie Typizowanej widoki dynamiczne](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) Aby uzyskać więcej informacji. 

Źródło, które były rodzaju z &quot;M&quot; dla modelu, ale nie rodzaj bazy danych. Przyjrzyjmy się, co możemy samouczka jest i utworzyć bazę danych filmów.

>[!div class="step-by-step"]
[Poprzednie](adding-a-controller.md)
[dalej](adding-a-model.md)
