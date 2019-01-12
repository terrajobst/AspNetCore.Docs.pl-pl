---
title: Dodaj widok do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodawanie widoku na prostej aplikacji ASP.NET Core MVC
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: 321ffd6b0168d4befc950a58035d19561e879491
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249454"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a>Dodaj widok do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji zmodyfikujesz `HelloWorldController` klasy [Razor](xref:mvc/views/razor) widok plików klarownie hermetyzacji proces generowania odpowiedzi HTML do klienta.

Utworzysz widok pliku szablonu przy użyciu aparatu Razor. Szablony widoku razor mają *.cshtml* rozszerzenie pliku. Zapewniają prosty sposób tworzenia danych wyjściowych HTML przy użyciu C#.

Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony w klasie kontrolera. W `HelloWorldController` klasy, Zastąp `Index` metoda następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Powyższy kod wywołuje kontrolera <xref:Microsoft.AspNetCore.Mvc.Controller.View*> metody. Szablonu widoku jest używane do generowania odpowiedzi HTML. Metody kontrolera (znany także jako *metod akcji*), takie jak `Index` ogólnie zwracany przez metodę powyżej <xref:Microsoft.AspNetCore.Mvc.IActionResult> (lub klasą pochodną <xref:Microsoft.AspNetCore.Mvc.ActionResult>), nie jest typem, takich jak `string`.

## <a name="add-a-view"></a>Dodawanie widoku

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Kliknij prawym przyciskiem myszy *widoków* folder, a następnie **Dodaj > Nowy Folder** i nadaj folderowi *HelloWorld*.

* Kliknij prawym przyciskiem myszy *widoków/HelloWorld* folder, a następnie **Dodaj > Nowy element**.

* W **Dodaj nowy element - MvcMovie** okna dialogowego

  * W polu wyszukiwania w prawym górnym rogu, wprowadź *widoku*

  * Wybierz **widoku Razor**

  * Zachowaj **nazwa** polu wartość *Index.cshtml*.

  * Wybierz **Dodaj**

![Dodaj okno dialogowe Nowy element](adding-view/_static/add_view.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Dodaj `Index` wyświetlić `HelloWorldController`.

* Dodaj nowy folder o nazwie *widoków/HelloWorld*.
* Dodaj nowy plik do *widoków/HelloWorld* nazwa folderu *Index.cshtml*.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *widoków* folder, a następnie **Dodaj > Nowy Folder** i nadaj folderowi *HelloWorld*.
* Kliknij prawym przyciskiem myszy *widoków/HelloWorld* folder, a następnie **Dodaj > Nowy plik**.
* W **nowy plik** okno dialogowe:

  * Wybierz **Web** w okienku po lewej stronie.
  * Wybierz **plik HTML pusty** w środkowym okienku.
  * Typ *Index.cshtml* w **nazwa** pole.
  * Wybierz **nowe**.

![Dodaj okno dialogowe Nowy element](adding-view/_static/add_view.png)

---  
<!-- End of VS tabs -->

Zastąp zawartość *Views/HelloWorld/Index.cshtml* plik widoku Razor następującym kodem:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

Przejdź do adresu `https://localhost:xxxx/HelloWorld`. `Index` Method in Class metoda `HelloWorldController` nie znacznie; został uruchomiony instrukcji `return View();`, które określone metody należy używać pliku szablonu widoku do renderowania odpowiedzi do przeglądarki. Ponieważ nie zostały jawnie określić nazwę pliku szablonu widoku, MVC używa domyślnie *Index.cshtml* plik widoku w */widoków/HelloWorld* folderu. Na poniższym obrazie przedstawiono ciąg "Hello z naszych Wyświetl szablon"! zakodowane w widoku.

![Okno przeglądarki](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a>Zmień widoki i układ strony

Wybierz linki menu (**MvcMovie**, **Home**, i **zachowania**). Każda strona zawiera ten sam układ menu. Układ menu jest zaimplementowana w *Views/Shared/_Layout.cshtml* pliku. Otwórz *Views/Shared/_Layout.cshtml* pliku.

[Układ](xref:mvc/views/layout) Szablony umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie. Znajdź `@RenderBody()` wiersza. `RenderBody` jest symbolem zastępczym, gdzie wszystkie widoku specyficzne dla strony, możesz utworzyć pokaz, *opakowane* w stronę układu. Na przykład w przypadku wybrania **zachowania** linku **Views/Home/Privacy.cshtml** renderowania widoku w `RenderBody` metody.

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a>Zmień łącze tytuł, stopki i menu, w pliku układu

* W elementach tytułu i stopki zmienić `MvcMovie` do `Movie App`.
* Zmień element kotwicy `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` do `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.

Następujący kod przedstawia podświetlony zmiany:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24)]

W poprzednim znaczników `asp-area` [atrybut Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) został pominięty, ponieważ ta aplikacja nie używa [obszarów](xref:mvc/controllers/areas).

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

**Uwaga**: `Movies` Kontrolera nie została zaimplementowana. W tym momencie `Movie App` link nie działa.

Zapisać zmiany i wybierz **zachowania** łącza. Zwróć uwagę, jak Wyświetla tytuł, na karcie przeglądarki **ochrona prywatności — aplikacja filmu** zamiast **ochrona prywatności — filmu Mvc**:

![Karta Prywatność](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

Wybierz **Home** link i zwróć uwagę, tekst tytułu i zakotwiczenia także wyświetlać **aplikacja filmu**. Byliśmy w stanie wprowadzić zmianę, jeden raz w szablonie układ, i ma wszystkich stron w witrynie odzwierciedlają nowego tekstu linku i nowy tytuł.

Sprawdź *Views/_ViewStart.cshtml* pliku:

```HTML
@{
    Layout = "_Layout";
}
```

*Views/_ViewStart.cshtml* wiąże plik *Views/Shared/_Layout.cshtml* pliku do każdego widoku. `Layout` Właściwość można ustawić inny układ widoku lub ustaw go na `null` więc plik układu nie będą używane.

Zmienianie tytułu i `<h2>` elementu *Views/HelloWorld/Index.cshtml* Wyświetl plik:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

Tytuł i `<h2>` element są nieco inne w przypadku, aby było widać, które fragmentem kodu zmienia wyświetlania.

`ViewData["Title"] = "Movie List";` w kodzie powyżej zestawy `Title` właściwość `ViewData` słownik "Listę programów filmu". `Title` Właściwość jest używana w `<title>` elementu HTML na stronę układu:

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

Zapisać zmiany i przejdź do `https://localhost:xxxx/HelloWorld`. Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione. (Jeśli nie widzisz zmian w przeglądarce, prawdopodobnie przeglądasz zawartość z pamięci podręcznej. Naciśnij klawisze Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewData["Title"]` jest ustawiany *Index.cshtml* wyświetlić szablonu i dodatkowych "-Movie App" dodane w pliku układu.

Należy również zauważyć, jak zawartości *Index.cshtml* szablon widoku została scalona z *Views/Shared/_Layout.cshtml* Wyświetl szablon i pojedynczą odpowiedź HTML był wysyłany do przeglądarki. Układ Szablony ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji. Aby dowiedzieć się więcej, zobacz [układ](xref:mvc/views/layout).

![Widok listy filmu](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

Nasze trochę "dane" (w tym przypadku "Hello z naszych Wyświetl szablon!" komunikat) jest ustalony, mimo że. Aplikacja MVC ma "V" (view) i masz "C" (kontroler), ale nie "M" (model) jeszcze.

## <a name="passing-data-from-the-controller-to-the-view"></a>Przekazywanie danych z kontrolera do widoku

Akcji kontrolera są wywoływane w odpowiedzi na przychodzące żądanie adresu URL. Klasa kontrolera jest, gdzie są zapisywane kod obsługujący żądania przychodzące przeglądarki. Kontroler pobiera dane ze źródła danych i decyduje o rodzaju odpowiedzi do odesłania do przeglądarki. Wyświetlanie szablonów może służyć za pomocą kontrolera do generowania i formatowanie odpowiedzi HTML do przeglądarki.

Kontrolery są odpowiedzialne za świadczenie danych wymagane dla szablonu widoku do renderowania odpowiedzi. Najlepszym rozwiązaniem jest: Wyświetl szablony powinny **nie** wykonania logiki biznesowej lub bezpośredniej interakcji z bazą danych. Przeciwnie Wyświetl szablon powinny działać tylko w przypadku danych, który znajduje się do niego przez kontroler. Obsługa tego "separacji" pomaga kod czystego, zakresie testować i obsługiwać.

Obecnie `Welcome` method in Class metoda `HelloWorldController` klasy przyjmuje `name` i `ID` parametru, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki. Zamiast kontrolera renderowania tej odpowiedzi jako ciąg, Zmień kontroler zamiast tego użyć szablonu widoku. Wyświetl szablon generuje odpowiedzi dynamicznej, co oznacza, że odpowiednich bitów danych muszą być przekazywane z kontrolera do widoku w celu wygenerowania odpowiedzi. W tym celu o kontrolera, umieść dane dynamiczne (parametry) wymagającym szablon widoku w `ViewData` słownik, który szablon widoku może uzyskać dostęp.

W *HelloWorldController.cs*, zmień `Welcome` metody w celu dodania `Message` i `NumTimes` wartość `ViewData` słownika. `ViewData` Słownik jest obiekt dynamiczny, co oznacza, można użyć dowolnego typu; `ViewData` obiekt nie ma zdefiniowanej właściwości do momentu coś znajdującym się w nim umieścić. [System powiązań modelu MVC](xref:mvc/models/model-binding) automatycznie mapuje nazwanych parametrów (`name` i `numTimes`) z ciągu zapytania w pasku adresu do parametrów w metodzie. Pełne *HelloWorldController.cs* pliku wygląda następująco:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

`ViewData` Obiekt słownika zawiera dane, które zostaną przekazane do widoku. 

Tworzenie szablonu-Zapraszamy wyświetlanie o nazwie *Views/HelloWorld/Welcome.cshtml*.

Utworzysz pętlę w *Welcome.cshtml* Wyświetl szablon, który wyświetla "Hello" `NumTimes`. Zastąp zawartość *Views/HelloWorld/Welcome.cshtml* następującym kodem:

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

Zapisz zmiany i przejdź do następującego adresu URL:

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

Dane są pobierane z adresu URL i przekazywane do kontrolera, za pomocą [integratora modelu MVC](xref:mvc/models/model-binding) . Kontroler pakiety danych do `ViewData` słownik i przebiegi, które obiekt widoku. Widok następnie renderuje dane jako kod HTML do przeglądarki.

![Widok zachowania powitalnej etykiety i frazy Hello Rick przedstawiono cztery razy](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

W przykładzie powyżej `ViewData` słownika zostało użyte do przekazywania danych z kontrolera do widoku. W dalszej części samouczka model widoku jest używany do przekazywania danych za pomocą kontrolera do widoku. Widok modelu sposobem przekazywania danych jest zwykle znacznie preferowany nad `ViewData` podejście słownika. Zobacz [kiedy używać elementów ViewBag, ViewData i TempData ](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) Aby uzyskać więcej informacji.

W następnym samouczku utworzeniu bazy danych filmów.

> [!div class="step-by-step"]
> [Poprzednie](adding-controller.md)
> [dalej](adding-model.md)
